[<- widgets](https://github.com/copycat-killer/lain/wiki/Widgets)

Shows in a textbox the current prices of Bitcoin to USD and Dogecoin to USD using Coinbase and Cryptsy's APIs. You will need [dkjson](http://dkolf.de/src/dkjson-lua.fsl/home). A simple way of installing is to download the dksjon.lua file and place it in the acw directory.

	acwwidget = lain.widgets.contrib.acw()

### input table

Variable | Meaning | Type | Default
--- | --- | --- | ---
`settings` | User settings | function | empty function

### output

A textbox.