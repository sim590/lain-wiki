Shows disk space usage for a set partition.

Displays a notification when the partition is full or has low space.

```lua
mypartition = lain.widgets.fs()
```

### Input table

Variable | Meaning | Type | Default
--- | --- | --- | ---
`timeout` | Refresh timeout seconds -| int | 600
`partition` | Partition to monitor | string | "/"
`notification_preset` | Notification preset | table | `{ fg = beautiful.fg_normal }`
`followtag` | Display the notification on currently focused screen | boolean | false
`notify` | Display "partition is empty" notifications | string | "on"
`showpopup` | Display pop-up window with df output | string | "on"
`settings` | User settings | function | empty function

`settings` can use the following `partition` related float values: `fs_now.used`, `fs_now.available`, `fs_now.size_mb`, `fs_now.size_gb`.

Within `settings`, you can obtain other partition values from internal `fs_info` table. For each partition, there are four index:

*  `fs_info[other_partition .. " used_p"]`
*  `fs_info[other_partition .. " avail_p"]`
*  `fs_info[other_partition .. " size_mb"]`
*  `fs_info[other_partition .. " size_gb"]`

just like the variables of `fs_now`. See [here](https://github.com/copycat-killer/lain/issues/103) for an usage example.

Also, `settings` can modify `fs_notification_preset` table. This table will be the preset for the naughty notifications. Check [here](http://awesome.naquadah.org/doc/api/modules/naughty.html#notify) for the list of variables it can contain. Default definition:

```lua
fs_notification_preset = { fg = beautiful.fg_normal }
```

In multiple screen setups, the default behaviour is to show a visual notification pop-up window on the first screen. By setting `followtag` to `true` it will be shown on the currently focused tag screen.

### Output table

Variable | Meaning | Type
--- | --- | ---
`widget` | The widget | `wibox.widget.textbox`
`show` | The notification | function

You can display the notification with a key binding like this:

```lua
awful.key({ altkey }, "h", function () mypartition.show(seconds, args, scr) end),
```

where ``altkey = "Mod1"`` and ``show`` arguments, all optionals, are:

* `seconds`, notification time in seconds;
* `options`, additional options to pass to [`dfs`](https://github.com/copycat-killer/lain/blob/master/scripts/dfs), in the form `--type='fstype' | --exclude-type='fstype'`;
* `scr`, screen in which display the notification.

If unsure or don't want to set an an argument, set it to `nil`. For instance, if you want to set 5 timeout seconds and screen 1: `mypartition.show(5, nil, 1)`.


**Note that** naughty notification requires `beautiful.font` or `fs_notification_preset.font` to be monospaced, in order to correctly display the output.
