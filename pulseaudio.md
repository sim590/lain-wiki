[<- widgets](https://github.com/copycat-killer/lain/wiki/Widgets)

Shows and controls PulseAudio volume with a textbox.

	volumewidget = lain.widgets.pulseaudio()

### input table

Variable | Meaning | Type | Default
--- | --- | --- | ---
`timeout` | Refresh timeout seconds | int | 5
`sink` | PulseAudio sink index | int | 0 
`cmd` | PulseAudio command | string | `"pacmd list-sinks | grep -e 'index: #SINK' -e 'volume: front' -e 'muted'"`
`settings` | User settings | function | empty function

`cmd` is useful in case you want to use a custom fetch command.

`settings` can use the following variables:

Variable | Meaning | Type | Values
--- | --- | --- | ---
`volume_now.left` | Front left level | int | 0-100
`volume_now.right` | Front right level | int | 0-100
`volume_now.muted` | Sink mute status | string | "yes", "no"

### output table

Variable | Meaning | Type
--- | --- | --- 
`widget` | The widget | `wibox.widget.textbox`
`sink` | PulseAudio sink | int
`update` | Update `widget` | function

You can control the widget with key bindings like these:

```lua
    -- PulseAudio volume control
    awful.key({ altkey }, "Up",
        function ()
            os.execute(string.format("pactl set-sink-volume %d +1%", volumewidget.sink))
            volumewidget.update()
        end),
    awful.key({ altkey }, "Down",
        function ()
            os.execute(string.format("pactl set-sink-volume %d -1%", volumewidget.sink))
            volumewidget.update()
        end),
    awful.key({ altkey }, "m",
        function ()
            if volumewidget.muted == "yes" then
                os.execute(string.format("pactl set-sink-mute %d no", volumewidget.sink))
            else
                os.execute(string.format("pactl set-sink-mute %d yes", volumewidget.sink))
            end
            volumewidget.update()
        end),
```

where `altkey = "Mod1"`.