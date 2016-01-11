A pulseaudio widget. It's using pacmd and pactl.

Shows and controls pulseaudio volume with a textbox.
```lua
volpulse = lain.widgets.contrib.pulseaudio()
```

Variable | Meaning | Type | Values
--- | --- | --- | ---
`volume_now.level` | Volume level | int | 0-100
`volume_now.status` | Mute status | string | "on", "off"

Available functions

Function | Meaning | Output Value  
--- | --- | ---
`volpulse.up()` | To increase a sound by 10 percent  | Volume level 
`volpulse.down()` | To decrease a sound by 10 percent | Volume level
`volpulse.toggle()` | Mute or unmute volume | Volume level

You can customize widget
```lua
volicon = wibox.widget.imagebox()
volpulse = lain.widgets.contrib.pulseaudio({
                                      settings = function()
                                         if volume_now.status == "off" then
                                            volicon:set_image(VOLUME_MUTEOFF_ICON)
                                         else
                                            volicon:set_image(VOLUME_ICON)
                                         end
                                      end
                                   })
```

You can control the widget with key bindings like these:

```lua
  -- Configure the hotkeys of PulseAudio.
   awful.key({ }, "XF86AudioRaiseVolume", function () volpulse.up() end ),
   
   awful.key({ }, "XF86AudioLowerVolume", function () volpulse.down() end ),
   awful.key({ }, "XF86AudioMute", function () volpulse.toggle() end)
```