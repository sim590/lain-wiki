Quake
-----

A Quake-like dropdown container for your favourite application.

**Usage**

Define it globally to have a single instance for all screens:

```lua
local quake = lain.util.quake()
```

or define it in `connect_for_each_screen` to have one instance for each screen:

```lua
awful.screen.connect_for_each_screen(function(s)
    -- Quake application
    s.quake = lain.util.quake()
    -- [...]
```

**Keybinding example**

If using a global instance:
```lua
awful.key({ modkey, }, "z", function () quake:toggle() end),
```

If using a per-screen instance:
```lua
awful.key({ modkey, }, "z", function () awful.screen.focused().quake:toggle() end),
```

**Input table**

Variable | Meaning | Type | Default
--- | --- | --- | ---
`app` | client to spawn | string | "xterm"
`name` | client name | string | "QuakeDD"
`argname` | how to specify client name | string | "-name %s"
`extra` | extra `app` arguments | string | empty string
`border` | border width | integer | 1
`visible` | initially visible | boolean | false
`followtag` | always spawn on currently focused screen | boolean | false
`overlap` | Overlap the wibox or not | boolean | false
`settings` | Additional settings to make on the client | function | `nil`
`screen` | screen where to spawn the client | integer | `awful.screen.focused()`
`height` | dropdown client height | float in [0,1] or exact pixels number | 0.25
`width` | dropdown client width | float in [0,1] or exact pixels number | 1
`vert` | vertical position | string, possible values: "top", "bottom", "center" | "top"
`horiz` | horizontal position | string, possible values: "left", "right", "center" | "left"

`height` and `width` express a fraction of the workspace.

`settings` is a function which takes the client as input, and can be used to customize its properties. For instance:

```lua
-- set the client sticky
s.quake = lain.util.quake { settings = function(c) c.sticky = true end }
```

Read [here](https://awesomewm.org/doc/api/classes/client.html#Object_properties) for the complete list of properties.

**Notes**

* [Does not work](https://github.com/lcpz/lain/issues/358) with `gnome-terminal`, `konsole`, or any other terminal which is strictly designed for a Desktop Environment. Just pick a better terminal, [there's plenty](https://wiki.archlinux.org/index.php/List_of_applications#Terminal_emulators).
* Set `followtag = true` if [experiencing issues with multiple screens](https://github.com/lcpz/lain/issues/346).
* If you have a `awful.client.setslave` rule for your application, ensure you use an exception for `QuakeDD` (or your defined `name`). Otherwise, you may run into problems with focus.
* If you are using a VTE-based terminal like `termite`, be sure to set [`argname = "--name %s"`](https://github.com/lcpz/lain/issues/211).

Separators
----------

Adds Cairo separators.

```lua
local separators = lain.util.separators
```

A separator function `separators.separator` takes two color arguments, defined as strings. `"alpha"` argument is allowed. Example:

```lua
arrl_dl = separators.arrow_left(beautiful.bg_focus, "alpha")
arrl_ld = separators.arrow_left("alpha", beautiful.bg_focus)
```

You can customize height and width by setting `separators_height` and `separators_width` in your `theme.lua`. Default values are 0 and 9, respectively.

List of functions:

     +-- separators
     |
     |`-- arrow_right()    Draw a right arrow.
      `-- arrow_left()     Draw a left arrow.

markup
------

Mades markup easier.

```lua
local markup = lain.util.markup
```

List of functions:

     +-- markup
     |
     |`-- bold()        Set bold.
     |`-- italic()      Set italicized text.
     |`-- strike()      Set strikethrough text.
     |`-- underline()   Set underlined text.
     |`-- monospace()   Set monospaced text.
     |`-- big()         Set bigger text.
     |`-- small()       Set smaller text.
     |`-- font()        Set the font of the text.
     |`-- font()        Set the font of the text.
     |`-- color()       Set background and foreground color.
     |`-- fontfg()      Set font and foreground color.
     |`-- fontbg()      Set font and background color.
      `-- fontcolor()   Set font, plus background and foreground colors.
     |
     |`--+ bg
     |   |
     |    `-- color()   Set background color.
     |
      `--+ fg
         |
          `-- color()   Set foreground color.

they all take one argument, which is the text to markup, except the following:

```lua
markup.font(font, text)
markup.color(fg, bg, text)
markup.fontfg(font, fg, text)
markup.fontbg(font, bg, text)
markup.fontcolor(font, fg, bg, text)
markup.fg.color(color, text)
markup.bg.color(color, text)
```

Dynamic tagging
---------------

That is:

- add a new tag;
- rename current tag;
- move current tag;
- delete current tag.

If you delete a tag, any rule set on it shall be broken, so be careful.

Use it with key bindings like these:

```lua
awful.key({ modkey, "Shift" }, "n", function () lain.util.add_tag(mylayout) end),
awful.key({ modkey, "Shift" }, "r", function () lain.util.rename_tag() end),
awful.key({ modkey, "Shift" }, "Left", function () lain.util.move_tag(1) end),   -- move to next tag
awful.key({ modkey, "Shift" }, "Right", function () lain.util.move_tag(-1) end), -- move to previous tag
awful.key({ modkey, "Shift" }, "d", function () lain.util.delete_tag() end),
```

The argument in `lain.util.add_tag` represents the tag layout, and is optional: if not present, it will be defaulted to `awful.layout.suit.tile`.

Useless gaps resize
---------------------

Changes `beautiful.useless_gaps` on the fly.

The function takes an integer argument, being the amount of pixel to add/remove to gaps.

You could use it with these keybindings:

```lua
-- On the fly useless gaps change
awful.key({ altkey, "Control" }, "+", function () lain.util.useless_gaps_resize(1) end),
awful.key({ altkey, "Control" }, "-", function () lain.util.useless_gaps_resize(-1) end),
```

where `altkey = Mod1`, or you could use it like this:

```lua
mywidget:buttons(awful.util.table.join (
        awful.button({}, 4, function() lain.util.useless_gaps_resize(-1) end),
        awful.button({}, 5, function() lain.util.useless_gaps_resize(1) end)
    end)
))
```

so when hovering the mouse over `mywidget`, you can adjust useless gaps size by scrolling with the mouse wheel.

tag\_view\_nonempty
-------------------

This function lets you jump to the next/previous non-empty tag.
It takes two arguments:

* `direction`: `1` for next non-empty tag, `-1` for previous.
* `sc`: Screen which the taglist is in. Default is `mouse.screen` or `1`. This
  argument is optional.

You can use it with key bindings like these:

```lua
-- Non-empty tag browsing
awful.key({ altkey }, "Left", function () lain.util.tag_view_nonempty(-1) end),
awful.key({ altkey }, "Right", function () lain.util.tag_view_nonempty(1) end),
```

where `altkey = "Mod1"`.

magnify\_client
---------------

Set a client to floating and resize it in the same way the "magnifier"
layout does it. Place it on the "current" screen (derived from the mouse
position). This allows you to magnify any client you wish, regardless of
the currently used layout. Use it with a client keybinding like this:

```lua
clientkeys = awful.util.table.join(
    -- [...]
    awful.key({ modkey, "Control" }, "m", lain.util.magnify_client),
    -- [...]
)
```

If you want to "de-magnify" it, just retype the keybinding.

If you want magnified client to respond to `incmwfact`, read [here](https://github.com/lcpz/lain/issues/195).

menu\_clients\_current\_tags
----------------------------

Similar to `awful.menu.clients`, but this menu only shows the clients
of currently visible tags. Use it with a key binding like this:

```lua
awful.key({ "Mod1" }, "Tab", function()
    lain.util.menu_clients_current_tags({ width = 350 }, { keygrabber = true })
end),
```

menu\_iterator
--------------

A generic menu utility which enables iteration over lists of possible
actions to execute. The perfect example is a simple menu for choosing what 
configuration to apply to X with `xrandr`, as suggested on the [awesome wiki page][xrandr-awesomewiki].

<p align="center">
    <img src="img/xrandr.gif"/>
    <br><b>NOTE:</b> above menu is <a href="https://github.com/sim590/dotfiles/blob/master/awesome/rc/xrandr.lua#L52">custom</a>
</p>

You can either create you own menu which has the following form:

```
{ { "choice description", callback } }
```

or use `util.menu_iterator.menu`, a generic menu creator.
Below is a table explaining the content of the menu function
arguments. The arguments are supplied as a table with named members.

**lain.menu_iterator.iterator**

| Argument  | Description                                                                           |
|-----------|---------------------------------------------------------------------------------------|
| `menu`    | The menu to iterate on.                                                               |
| `timeout` | The time (in seconds) to wait on a choice before the choice is accepted (default: 4). |
| `icon`    | The path to the icon to display in `naughty.notify` window.                           |

**lain.menu_iterator.menu**

<table>
    <tr>
        <td align="center"><b>Argument</b></td>
        <td align="center"><b>Description</b></td>
    </tr>
    <tr>
        <td><code>choices</code></td>
        <td>A list of choices as string elements <code>{"choice1", "choice2", ...}</code></td>
    </tr>
    <tr>
        <td><code>name</code></td>
        <td>The name of the program concerned by this menu interface</td>
    </tr>
    <tr>
        <td><code>selected_cb</code></td>
        <td>The function to call for all selected choices through the menu. The
        function is passed the choice of type string as parameter.</td>
    </tr>
    <tr>
        <td><code>rejected_cb</code></td>
        <td>The function to call for all rejected choices through the menu. The
        function is passed the choice of type string as parameter</td>
    </tr>
    <tr>
        <td><code>extra_choices</code></td>
        <td>More choices to be added to the menu. The difference with those
        choice is that they won't trigger any other action for rejected
        choices.</td>
    </tr>
    <tr>
        <td><code>combination</code></td>
        <td>Can either be "powerset" or "single" (the default). The former will
        generate a menu with choices that are the [powerset][] of the choices
        given by the argument `choices`. The latter will simply take choices as
        they come.</td>
    </tr>
</table>

[powerset]: https://en.wikipedia.org/wiki/Power_set

**Examples**

First, include the menu iterator module:

```lua
local menu_iterator = require("lain.util.menu_iterator")
```

The simplest example is as follows:

```lua
local choices = {"My first choice", "My second choice"}
local m = util.menu({
    choices     = choices,
    name        = "My awesome program",
    selected_cb = function(choice)
        -- do something with this with the user choice
    end,
    rejected_cb = function(choice)
        -- do something with this rejected choice
    end
})
```

The call to the function `menu` will return a menu compatible with the function
`iterate` in order to iterate on it using the `naughty.notify` utility. To do
so, one has to write something like the following:

```lua
-- m defined above
time_to_chose = 4
my_awesome_icon = "/the/path/to/the/icon"
menu_iterator.iterate(m, time_to_chose, my_awesome_icon)
```

Subsequent calls to the `iterate` function will step further into the list of
choice to actually iterate over the list. Once the timeout has passed (4 seconds
by default) without anymore calls to `iterate`, the choice is made and it calls
the associated callbacks (for selected and rejected choices) configured in the
menu.

One can add a `Cancel` option which would do as it says: it would cancel the
menu selection. Simply write:

```lua
local m = util.menu({
    choices     = {"One thing", "along with this thing"},
    name        = "My awesome program",
    selected_cb = function(choice)
        -- do something with this with the user choice
    end,
    rejected_cb = function(choice)
        -- do something with this rejected choice
    end,
    -- We pass nil because there's nothing to do in this case. It is not forced
    -- though. One could pass something to do on the "Cancel" choice of course.
    extra_choices = { {"Cancel"}, nil },
    combination = "powerset"
})
```

In this last example, `powerset` has been set along with the set of choices
`{"One thing", "along with this thing"}` which created the following powerset:

```
{{"One thing"}, {"along with this thing"}, { "One thing", "along with this thing" }}
```

[xrandr-awesomewiki]: https://awesomewm.org/recipes/xrandr/
