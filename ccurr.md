Shows in a textbox the current prices of Bitcoin to USD using [Coinbase V1 API](https://developers.coinbase.com/api/v1).

```lua
ccurrwidget = lain.widgets.contrib.ccurr()
```

### input table

Variable | Meaning | Type | Default
--- | --- | --- | ---
`timeout` | Refresh timeout seconds | int | 600
`btc_url` | URL to Json Bitcoin data | string | Coinbase V1 API
`settings` | User settings | function | empty function

`settings` can use the `price_now` table, which contains the following strings:

- `btc`

### output

A textbox.