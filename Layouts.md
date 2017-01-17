
    lain/layout
    .
    |-- termfair
    |-- termfair.center
    |-- cascade
    |-- cascade.tile
    |-- centerwork
    |-- centerwork.horizontal
    |-- centerworkd

Usage
=====

Just specify your favourites like usual, or set them on specific tags like this:

```lua
awful.layout.set(lain.layout.termfair, tag)
```

How do layouts work?
====================

`termfair`
--------

I do a lot of work on terminals. The common tiling algorithms usually
maximize windows, so you'll end up with a terminal that has about 200
columns or more. That's way too much. Have you ever read a manpage in a
terminal of this size?

This layout restricts the size of each window. Each window will have the
same width but is variable in height. Furthermore, windows are
left-aligned. The basic workflow is as follows (the number above the
screen is the number of open windows, the number in a cell is the fixed
number of a client):

	     (1)                (2)                (3)
	+---+---+---+      +---+---+---+      +---+---+---+
	|   |   |   |      |   |   |   |      |   |   |   |
	| 1 |   |   |  ->  | 2 | 1 |   |  ->  | 3 | 2 | 1 |  ->
	|   |   |   |      |   |   |   |      |   |   |   |
	+---+---+---+      +---+---+---+      +---+---+---+

	     (4)                (5)                (6)
	+---+---+---+      +---+---+---+      +---+---+---+
	| 4 |   |   |      | 5 | 4 |   |      | 6 | 5 | 4 |
	+---+---+---+  ->  +---+---+---+  ->  +---+---+---+
	| 3 | 2 | 1 |      | 3 | 2 | 1 |      | 3 | 2 | 1 |
	+---+---+---+      +---+---+---+      +---+---+---+

The first client will be located in the left column. When opening
another window, this new window will be placed in the left column while
moving the first window into the middle column. Once a row is full,
another row above it will be created.

Default number of columns and rows are respectively taken from `nmaster`
and `ncol` values in `awful.tag`, but you can set your own.

For example, this sets `termfair` to 3 columns and at least 1 row:

```lua
lain.layout.termfair.nmaster = 3
lain.layout.termfair.ncol    = 1
```

You can also set a global border:

```lua
lain.layout.termfair.global_border_width = 1 -- default is 0
```

`termfair.center`
----------

Similar to `termfair`, but with fixed number of vertical columns. Cols are centerded until there are `nmaster` columns, then windows are stacked as slaves, with possibly `ncol` clients per column at most.

            (1)                (2)                (3)
       +---+---+---+      +-+---+---+-+      +---+---+---+
       |   |   |   |      | |   |   | |      |   |   |   |
       |   | 1 |   |  ->  | | 1 | 2 | | ->   | 1 | 2 | 3 |  ->
       |   |   |   |      | |   |   | |      |   |   |   |
       +---+---+---+      +-+---+---+-+      +---+---+---+

            (4)                (5)
       +---+---+---+      +---+---+---+
       |   |   | 3 |      |   | 2 | 4 |
       + 1 + 2 +---+  ->  + 1 +---+---+
       |   |   | 4 |      |   | 3 | 5 |
       +---+---+---+      +---+---+---+

Like `termfair`, default number of columns and rows are respectively taken from `nmaster`
and `ncol` values in `awful.tag`, but you can set your own.

For example, this sets `termfair.center` to 3 columns and at least 1 row:

```lua
lain.layout.termfair.center.nmaster = 3
lain.layout.termfair.center.ncol    = 1

Also, `lain.layout.termfair.global_border_width` will be applied here too.

`cascade`
-------

Cascade all windows of a tag.

You can control the offsets by setting these two variables:

```lua
lain.layout.cascade.offset_x = 64
lain.layout.cascade.offset_y = 16
```

The following reserves space for 5 windows:

```lua
lain.layout.cascade.nmaster = 5
```

That is, no window will get resized upon the creation of a new window,
unless there's more than 5 windows.

You can also set a global border:

```lua
lain.layout.cascade.global_border_width = 1 -- default is 0
```

`cascade.tile`
-----------

Similar to `awful.layout.suit.tile` layout, however, clients in the slave
column are cascaded instead of tiled.

Left column size can be set, otherwise is controlled by `mwfact` of the
tag. Additional windows will be opened in another column on the right.
New windows are placed above old windows.

Whether the slave column is placed on top of the master window or not is
controlled by the value of `ncol`. A value of 1 means "overlapping slave column"
and anything else means "don't overlap windows".

Usage example:

```lua
lain.layout.cascade.tile.offset_x      = 2
lain.layout.cascade.tile.offset_y      = 32
lain.layout.cascade.tile.extra_padding = 5
lain.layout.cascade.tile.nmaster       = 5
lain.layout.cascade.tile.ncol          = 1
```

`extra_padding` reduces the size of the master window if "overlapping
slave column" is activated. This allows you to see if there are any
windows in your slave column.

Setting `offset_x` to a very small value or even 0 is recommended to avoid wasting space.

Also, `lain.layout.cascade.global_border_width` will be applied here too.

`centerwork`
----------

You start with one window, centered horizontally:

	+--------------------------+
	|       +----------+       |
	|       |          |       |
	|       |          |       |
	|       |          |       |
	|       |   MAIN   |       |
	|       |          |       |
	|       |          |       |
	|       |          |       |
	|       |          |       |
	|       +----------+       |
	+--------------------------+

This is your main working window. You do most of the work right here.
Sometimes, you may want to open up additional windows. They're put in
the following four slots:

	+--------------------------+
	| +---+ +----------+ +---+ |
	| |   | |          | |   | |
	| | 0 | |          | | 1 | |
	| |   | |          | |   | |
	| +---+ |   MAIN   | +---+ |
	| +---+ |          | +---+ |
	| |   | |          | |   | |
	| | 2 | |          | | 3 | |
	| |   | |          | |   | |
	| +---+ +----------+ +---+ |
	+--------------------------+

Yes, the number "four" is fixed. In total, you can only have five open
windows with this layout. Additional windows are not managed and set to
floating mode. **This is intentional**.

You can set the order of the four auxiliary windows. This is the default
configuration:

```lua
lain.layout.centerwork.top_left     = 0
lain.layout.centerwork.top_right    = 1
lain.layout.centerwork.bottom_left  = 2
lain.layout.centerwork.bottom_right = 3
```

This means: The bottom left slot will be occupied by the third window
(not counting the main window). Suppose you want your windows to appear
in this order:

	+--------------------------+
	| +---+ +----------+ +---+ |
	| |   | |          | |   | |
	| | 3 | |          | | 0 | |
	| |   | |          | |   | |
	| +---+ |   MAIN   | +---+ |
	| +---+ |          | +---+ |
	| |   | |          | |   | |
	| | 2 | |          | | 1 | |
	| |   | |          | |   | |
	| +---+ +----------+ +---+ |
	+--------------------------+

This would require you to use these settings:

```lua
lain.layout.centerwork.top_left     = 3
lain.layout.centerwork.top_right    = 0
lain.layout.centerwork.bottom_left  = 2
lain.layout.centerwork.bottom_right = 1
```

*Please note:* If you use Awesome's default configuration, navigation in
this layout may be very confusing. How do you get from the main window
to satellite ones depends on the order in which the windows are opened.
Thus, use of `awful.client.focus.bydirection()` is suggested.
Here's an example:

```lua
globalkeys = awful.util.table.join(
    -- [...]
    awful.key({ modkey }, "j",
        function()
            awful.client.focus.bydirection("down")
            if client.focus then client.focus:raise() end
        end),
    awful.key({ modkey }, "k",
        function()
            awful.client.focus.bydirection("up")
            if client.focus then client.focus:raise() end
        end),
    awful.key({ modkey }, "h",
        function()
            awful.client.focus.bydirection("left")
            if client.focus then client.focus:raise() end
        end),
    awful.key({ modkey }, "l",
        function()
            awful.client.focus.bydirection("right")
            if client.focus then client.focus:raise() end
        end),
    -- [...]
)
```

`centerwork.horizontal`
-----------

Same as `centerwork`, except that the main
window expands horizontally, and the 4 additional windows
are put ontop/below it, thus using the huge vertical space
much better. Useful if you have a screen turned 90°.

`centerworkd`
-----------

Same as `centerwork`, except that this version fills the slave-columns regardless of how many slave-clients are present.

Pre 4.0 `uselesstile` patches
=============================

Useless gaps have been integrated in Awesome 4.0, so the `useless*` layouts have been removed.

Following are a couple of `uselesstile` variants that were not part of lain: they are kept only for reference.

Xmonad-like
-----------

If you want to have `awful.layout.suit.tile` behave like xmonad, with internal gaps two times wider than external ones, download [this](https://gist.github.com/copycat-killer/9e56dcfbe66bfe14967c) as `lain/layout/uselesstile`.

Inverted master
---------------

Want to invert master window position? Use [this](https://gist.github.com/copycat-killer/c59dc59c9f99d98218eb) version. You can set `single_gap` with `width` and `height` in your `theme.lua`, in order to define the window geometry when there's only one client, otherwise it goes maximized. An example:

    theme.single_gap = { width = 600, height = 100 }

What about layout icons?
========================

They are located in ``lain/icons/layout``.

To use them, add lines to your ``theme.lua`` like this:

```lua
theme.lain_icons         = os.getenv("HOME") ..
                           "/.config/awesome/lain/icons/layout/default/"
theme.layout_termfair    = theme.lain_icons .. "termfair.png"
theme.layout_centerfair  = thema.lain_icons .. "centerfair.png"  -- termfair.center
theme.layout_cascade     = theme.lain_icons .. "cascade.png"
theme.layout_cascadetile = theme.lain_icons .. "cascadetile.png" -- cascade.tile
theme.layout_centerwork  = theme.lain_icons .. "centerwork.png"
theme.layout_centerhwork = theme.lain_icons .. "centerhwork.png" -- centerwork.horizontal
```

Credits go to [Nicolas Estibals](https://github.com/nestibal) for creating
layout icons for default theme.

You can use them as a template for your custom versions.