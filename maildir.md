## Usage

[Read here.](https://github.com/copycat-killer/lain/wiki/Widgets#usage)

### Description

Shows maildirs status.

Maildirs are structured as follows:

	~/Mail
	.
	|-- arch
	|   |-- cur
	|   |-- new
	|   `-- tmp
	|-- gmail
	|   |-- cur
	|   |-- new
	|   `-- tmp
	.
	.
	.

therefore the widget checks whether there are files in the `new` directories.
If there's new mails, the textbox will say something like "mail: bugs(3), system(1)", otherwise it says
"no mail".

```lua
local mymaildir = lain.widgets.maildir(args)
```

## Input table

Variable | Meaning | Type | Default
--- | --- | --- | ---
`timeout` | Refresh timeout seconds | int | 60
`mailpath` | Path to your maildir | string | "~/Mail"
`ignore_boxes` | Boxes to ignore | table of strings | empty table
`cmd` | External mail update command | string | empty string
`settings` | User settings | function | empty function

`settings` can use the string `newmail`, which format will be something like defined above, or "no mail".
`cmd` can be used to run a mail update command, for instance:

```lua
mailwidget = lain.widgets.maildir({
    cmd = "mbsync -q account1 account2 account3",
    -- [...]
})
```

## Output table

Variable | Meaning | Type
--- | --- | ---
`widget` | The widget | `wibox.widget.textbox`
`update` | Update `widget` | function
`timer` | The widget timer | [`gears.timer`](https://awesomewm.org/doc/api/classes/gears.timer.html)

The `update` function can be used to refresh the widget before `timeout` expires.

You can use `timer` to start/stop the widget as you like.