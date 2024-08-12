# About

This library is currently under development, though released versions are tested and should be stable.

The goal of this library is to create simple yet versatile interfaces to draw arrows on the screen. Because of the built in tick update mechanism the position and orientation of the arrows are also updated to follow moving targets.

**Roadmap**

-   Spaced tick update when tracking large amounts of arrows
-   Grouping arrows when multiple entities are within the same direction
-   Implement striked through parameters

# Wiki

## Initialization

```lua
local arrow = require("__arrowlib__/arrow")

script.on_init(function(e)
    --OPTION 1: Init without additional arguments
    arrow.init()

    --OPTION 2: Init with additional arguments
    local data = {
        time_to_live = 30, -- Override default arrow time to live to 30 seconds
    }
    arrow.init(data)
end)

script.on_event(defines.events.on_tick, function(e)
    arrow.tick_update()
end)
```

---

`init({...})`

Initializes the arrow library, which is required to be done from the mod using the library since the library utilizes the mod's `global`

**Parameters**

(arguments which are striked through are not yet implemented)

| Argument                 | Default | Accepted values                                                                                       | Description                                                                                           |
| ------------------------ | ------- | ----------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| `time_to_live` ?         | 0       | nil, 0 to 71 582 788                                                                                  | Default time to live in seconds for newly created arrows                                              |
| `offset` ?               | 5       | 1 to 50                                                                                               | Offset from source in tiles                                                                           |
| `scale` ?                | 2       | 1 to 25                                                                                               | Relative scaling of the arrow graphic                                                                 |
| ~~`updates_per_tick`~~ ? | 50      | 1 to 500                                                                                              | How many arrows per tick are to be updated                                                            |
| `raise_errors` ?         | true    | bool                                                                                                  | Wether critical incorrect function calls should raise an error or continue                            |
| `raise_warnings` ?       | false   | bool                                                                                                  | Wether non-destructive incorrect function calls should raise an error or continue                     |
| ~~`draw_target_box`~~ ?  | true    | bool                                                                                                  | Draw a rectangle around the target                                                                    |
| ~~`forces`~~ ?           | nil     | array[[ForceIdentification](https://lua-api.factorio.com/latest/concepts.html#ForceIdentification)]   | Forces that this arrow is rendered to, passing `nil` or an empty table will render it to all forces   |
| ~~`players`~~ ?          | nil     | array[[PlayerIdentification](https://lua-api.factorio.com/latest/concepts.html#PlayerIdentification)] | Players that this arrow is rendered to, passing `nil` or an empty table will render it to all players |

---

`tick_update()`

Updates the orientation and position of arrows, which is required to be done from the mod using the library since the library utilizes the mod's `global`

---

## Create new arrows

```lua
local function foo()
    --Get some variables to work with
    local player_one = game.players[1]
    local arrow_target = {0,0}
    local data = {
        source = player_one.character, -- Draw from the character
        target = arrow_target, -- Point to the world origin
        time_to_live = 30 -- Overrides this arrow's time to live for 30 seconds
    }

    --Draw the arrow and store the reference in global
    local id = arrow.create(data)
    global.foo.id = id
end
```

---

`create({...})` --> _string_ or false

Creates a new arrow

**Parameters**

(arguments which are striked through are not yet implemented)

| Argument                | Accepted values                                                                                       | Description                                                                                                     |
| ----------------------- | ----------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| `source` ?              | position data\*                                                                                       | Center of the source where the arrow revolves around                                                            |
| `source_direction` ?    | orientation\*\*                                                                                       | Required and only used when `source` is not set, defaults to `from_top`                                         |
| `target`                | position data\*                                                                                       | Center of the target where the arrow is pointing to                                                             |
| `surface`?              | [SurfaceIdentification](https://lua-api.factorio.com/latest/concepts.html#SurfaceIdentification)      | The arrow's surface, required if both `source` and `target` are MapPosition                                     |
| `time_to_live` ?        | nil, 0 to 71 582 788                                                                                  | Overrides the time to live in seconds for this specific arrow                                                   |
| `offset` ?              | 1 to 50                                                                                               | Overrides the offset from source (or target when `source` is not set) in tiles for this specific arrow          |
| `scale` ?               | 1 to 25                                                                                               | Overrides the scaling for this specific arrow                                                                   |
| ~~`draw_target_box` ?~~ | bool                                                                                                  | Overrides drawing a rectangle around this specific target                                                       |
| ~~`forces` ?~~          | array[[ForceIdentification](https://lua-api.factorio.com/latest/concepts.html#ForceIdentification)]   | Overrides forces that this arrow is rendered to, passing `nil` or an empty table will render it to all forces   |
| ~~`players` ?~~         | array[[PlayerIdentification](https://lua-api.factorio.com/latest/concepts.html#PlayerIdentification)] | Overrides players that this arrow is rendered to, passing `nil` or an empty table will render it to all players |
| ~~`render_tag_icon`~~   | Bool                                                                                                  | Render the tag icon on the world when pointing from/to a chart tag, defaults to `false`                         |

\*) Position data = union{[LuaEntity](https://lua-api.factorio.com/latest/classes/LuaEntity.html), [LuaCustomChartTag](https://lua-api.factorio.com/latest/classes/LuaCustomChartTag.html), [LuaTile](https://lua-api.factorio.com/latest/classes/LuaTile.html), [BoundingBox](https://lua-api.factorio.com/latest/concepts.html#BoundingBox) [MapPosition](https://lua-api.factorio.com/latest/concepts.html#MapPosition)}

\*\*) orientation = union{`arrow.defines.source_direction.from_top`, `arrow.defines.source_direction.from_left`, `arrow.defines.source_direction.from_bottom`, `arrow.defines.source_direction.from_right`}

---

## Remove existing arrows

```lua
local function bar()
    -- OPTION 1: Remove individual arrows
    local success = arrow.remove(global.foo.id)

    -- OPTION 2: Remove all arrows
    local success = arrow.remove_all()
end
```

---

`remove(id)` --> _bool_

Removes an existing arrow.

**Parameters**

| Argument | Accepted values | Description                  |
| -------- | --------------- | ---------------------------- |
| `id`     | string          | The ID of any existing arrow |

---

`remove_all()` --> _bool_

Removes all existing arrows.

---

# Collaborations welcome

-   Start a discussion with your ideas
-   Open a pull request on Github
-   Report issues under discussions
