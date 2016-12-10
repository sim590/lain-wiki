The [asynchronous](https://github.com/copycat-killer/lain/issues/128) version of [`base`](https://github.com/copycat-killer/lain/wiki/base):

```lua
myasyncbase = lain.widgets.abase()
```

Read [here](https://github.com/copycat-killer/lain/wiki/base) for the rest.

Use case examples
========

cmus
----

```lua
cmuswidget = lain.widgets.abase({
    cmd = "cmus-remote -Q",
    settings = function()
        cmus_now = {
            state   = "N/A",
            artist  = "N/A",
            title   = "N/A",
            album   = "N/A"
        }

        for w in string.gmatch(output, "(.-)tag") do
            a, b = w:match("(%w+) (.-)\n")
            cmus_now[a] = b
        end

        -- customize here
        widget:set_text(cmus_now.artist .. " - " .. cmus_now.title)
    end
})
```

bitcoin
-------

```lua
-- Bitcoin to USD current price, using Coinbase V1 API
bitcoinwidget = lain.widgets.abase({
    cmd = "curl -m5 -s 'https://coinbase.com/api/v1/prices/buy'",
    settings = function()
        local btc, pos, err = require("lain.util").dkjson.decode(output, 1, nil)
        local btc_price = (not err and btc and btc["subtotal"]["amount"]) or "N/A"

        -- customize here
        widget:set_text(btc_price)
    end
})
```

mpris
-----

```lua
-- Infos from mpris clients such as spotify and VLC
-- Based on https://github.com/acrisci/playerctl
mpriswidget = lain.widgets.abase({
    cmd = "playerctl status && playerctl metadata",
    settings = function()
         escape_f  = require("awful.util").escape
         mpris_now = {
             state        = "N/A",
             artist       = "N/A",
             title        = "N/A",
             art_url      = "N/A",
             album        = "N/A",
             album_artist = "N/A"
         }

         mpris_now.state = string.match(output, "Playing") or
                           string.match(output, "Paused")  or "N/A"

         for k, v in string.gmatch(output, "'[^:]+:([^']+)':[%s]<%[?'([^']+)'%]?>")
         do
             if     k == "artUrl"      then mpris_now.art_url      = v
             elseif k == "artist"      then mpris_now.artist       = escape_f(v)
             elseif k == "title"       then mpris_now.title        = escape_f(v)
             elseif k == "album"       then mpris_now.album        = escape_f(v)
             elseif k == "albumArtist" then mpris_now.album_artist = escape_f(v)
             end
         end

        -- customize here
        widget:set_text(mpris_now.artist .. " - " .. mpris_now.title)
    end
})
```