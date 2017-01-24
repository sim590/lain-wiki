This is a simple template widget.

Basically, all it does is to execute an input `cmd`, and to fill its textbox with the output.

```lua
mybase = lain.widgets.base()
```

### Input table

Variable | Meaning | Type | Default
--- | --- | --- | ---
`timeout` | Refresh timeout seconds | int | 5
`cmd` | The command to execute | string | empty string
`nostart` | Widget timer doesn't start immediately | boolean | false
`stoppable` | Widget timer is stoppable | boolean | false
`settings` | User settings | function | empty function

`settings` can use the string `output`, which is the output of `cmd`.

### Output table

Variable | Meaning | Type
--- | --- | ---
`widget` | The widget | `wibox.widget.textbox`
`update` | Update `widget` | function
`timer` | The widget timer | [`gears.timer`](https://awesomewm.org/doc/api/classes/gears.timer.html) or `nil`

The `update` function can be used to refresh the widget before `timeout` expires.

If `stoppable == true`, the widget will have an ad-hoc timer, which you can control though `timer` variable.