Shows ALSA volume with a progressbar; provides tooltips and notifications.

```lua
volume = lain.widgets.alsabar()
```

### Input table

The table and all of its variables are optional.

Variable | Meaning | Type | Default
--- | --- | --- | ---
`timeout` | Refresh timeout seconds | int | 5
`settings` | User settings | function | empty function
`width` | Bar width | int | 63
`height` | Bar height | int | 1
`ticks` | Set bar ticks on | boolean | false
`ticks_size` | Ticks size | int | 7
`vertical` | Set the bar vertical | boolean | false
`cmd` | ALSA mixer command | string | "amixer"
`channel` | Mixer channel | string | "Master"
`colors` | Bar colors | table | see [Default colors](https://github.com/copycat-killer/lain/wiki/alsabar#default-colors)
`notification_preset` | Notifications settings | table | [`naughty.config.defaults`](https://awesomewm.org/apidoc/libraries/naughty.html#config.defaults)
`notify` | Display notifications | string | "on"
`followtag` | Display the notification on currently focused screen | boolean | false

`cmd` is useful if you need to pass additional arguments to  `amixer`. For instance, users with multiple sound cards may define `command = "amixer -c X"` in order to set amixer with card `X`.

`settings` can use the following variables:

Variable | Meaning | Type | Values
--- | --- | --- | ---
`volume_now.level` | Self explained | int | 0-100
`volume_now.status` | Device status | string | "on", "off"

In multiple screen setups, the default behaviour is to show a visual notification pop-up window on the first screen. By setting `followtag` to `true` it will be shown on the currently focused tag screen.

### Default colors

Variable | Meaning | Type | Default
--- | --- | --- | ---
`background` | Bar backgrund color | string | "#000000"
`mute` | Bar mute color | string | "#EB8F8F"
`unmute` | Bar unmute color | string | "#A4CE8A"

### Output table

Variable | Meaning | Type
--- | --- | ---
`bar` | The widget | `wibox.widget.progressbar`
`channel` | Alsa channel | string
`notify` | The notification | function
`update` | Update state | function

### Keybindings

You can control the widget with keybindings like these:

```lua
-- ALSA volume control
awful.key({ altkey }, "Up",
	function ()
		os.execute(string.format("amixer set %s 1%%+", volume.channel))
		volume.update()
	end),
awful.key({ altkey }, "Down",
	function ()
		os.execute(string.format("amixer set %s 1%%-", volume.channel))
		volume.update()
	end),
awful.key({ altkey }, "m",
	function ()
		os.execute(string.format("amixer set %s toggle", volume.channel))
		volume.update()
	end),
awful.key({ altkey, "Control" }, "m",
	function ()
		os.execute(string.format("amixer set %s 100%%", volume.channel))
		volume.update()
	end),
awful.key({ altkey, "Control" }, "0",
	function ()
		os.execute(string.format("amixer set %s 0%%", volume.channel))
		volume.update()
	end),
```

where `altkey = "Mod1"`.