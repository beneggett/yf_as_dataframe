# Yfinrb

# Download market data from Yahoo! Finance's API

<table border=1 cellpadding=10><tr><td>

#### \*\*\* IMPORTANT LEGAL DISCLAIMER \*\*\*

---

**Yahoo!, Y!Finance, and Yahoo! finance are registered trademarks of
Yahoo, Inc.**

yfinrb is **not** affiliated, endorsed, or vetted by Yahoo, Inc. It is
an open-source tool that uses Yahoo's publicly available APIs, and is
intended for research and educational purposes.

**You should refer to Yahoo!'s terms of use**
([here](https://policies.yahoo.com/us/en/yahoo/terms/product-atos/apiforydn/index.htm),
[here](https://legal.yahoo.com/us/en/yahoo/terms/otos/index.html), and
[here](https://policies.yahoo.com/us/en/yahoo/terms/index.htm)) **for
details on your rights to use the actual data downloaded. Remember - the
Yahoo! finance API is intended for personal use only.**

</td></tr></table>

---

## Quick Start

### The Ticker module

The `Ticker` class, which allows you to access ticker data:

```ruby

msft = Yfinrb::Ticker.new("MSFT")

# get all stock info
msft.info

# get historical market data as a dataframe using [Polars](https://github.com/ankane/ruby-polars?tab=readme-ov-file)
hist = msft.history(period: "1mo")
hist2 = msft.history(start: '2020-01-01', fin: '2021-12-31')

# show meta information about the history (requires history() to be called first)
msft.history_metadata

# show actions (dividends, splits, capital gains)
msft.actions
msft.dividends
msft.splits
msft.capital_gains  # only for mutual funds & etfs

# show share count
msft.shares_full(start: "2022-01-01", fin: nil)

# show financials:
# - income statement
msft.income_stmt
msft.quarterly_income_stmt
# - balance sheet
msft.balance_sheet
msft.quarterly_balance_sheet
# - cash flow statement
msft.cashflow
msft.quarterly_cashflow

# show holders
msft.major_holders
msft.institutional_holders
msft.mutualfund_holders
msft.insider_transactions
msft.insider_purchases
msft.insider_roster_holders

# show recommendations
msft.recommendations
msft.recommendations_summary
msft.upgrades_downgrades

# Show future and historic earnings dates, returns at most next 4 quarters and last 8 quarters by default.
msft.earnings_dates

# show ISIN code
# ISIN = International Securities Identification Number
msft.isin

# show options expirations
msft.options

# show news
msft.news

# get option chain for specific expiration
opt = msft.option_chain('2026-12-18')
# data available via: opt.calls, opt.puts


# technical operations, using the Tulirb gem, which provides bindings to 
# the Tulip technical indicators library
h = msft.history(period: '2y', interval: '1d')
Yfinrb.ad(h)

# then
h.insert_at_idx(h.columns.length, Yfinrb.ad(h))
h['ad_results'] = Yfinrb.ad(h)

```

Most of the indicators [here](https://tulipindicators.org/) and [here](https://www.rubydoc.info/github/ozone4real/tulirb/main/Tulirb).  Indicator parameters in [Tulirb](https://www.rubydoc.info/github/ozone4real/tulirb/main/Tulirb) called, e.g., "period" or "short_period" are renamed as "window" or "short_window", respectively.  There are a few other variants that are affected.  Default values are shown below.

```ruby

df = msft.history(period: '3y', interval: '1d') # for example

Yfinrb.ad(df)
Yfinrb.adosc(df, short_window: 2, long_window: 5)
Yfinrb.adx(df, column: 'Adj Close', window: 5)
Yfinrb.adxr(df, column: 'Adj Close', window: 5)
Yfinrb.avg_daily_trading_volume(df, window: 20)
Yfinrb.ao(df)
Yfinrb.apo(df, column: 'Adj Close', short_window: 12, long_window: 29)
Yfinrb.aroon(df, window: 20)
Yfinrb.aroonosc(df, window: 20)
Yfinrb.avg_price(df)
Yfinrb.atr(df, window: 20)
Yfinrb.bbands(df, column: 'Adj Close', window: 20, stddev: 1 )
Yfinrb.bop(df)
Yfinrb.cci(df, window: 20)
Yfinrb.cmo(df, column: 'Adj Close', window: 20)
Yfinrb.cvi(df, window: 20)
Yfinrb.dema(df, column: 'Adj Close', window: 20)
Yfinrb.di(df, window: 20)
Yfinrb.dm(df, window: 20)
Yfinrb.dpo(df, column: 'Adj Close', window: 20)
Yfinrb.dx(df, window: 20)
Yfinrb.ema(df, column: 'Adj Close', window: 5) 
Yfinrb.emv(df)
Yfinrb.fisher(df, window: 20) 
Yfinrb.fosc(df, window: 20) 
Yfinrb.hma(df, column: 'Adj Close', window: 5) 
Yfinrb.kama(df, column: 'Adj Close', window: 5)
Yfinrb.kvo(df, short_window: 5, long_window: 20)
Yfinrb.linreg(df, column: 'Adj Close', window: 20)
Yfinrb.linregintercept(df, column: 'Adj Close', window: 20)
Yfinrb.linregslope(df, column: 'Adj Close', window: 20)
Yfinrb.macd(df, column: 'Adj Close', short_window: 12, long_window: 26, signal_window: 9)
Yfinrb.marketfi(df)
Yfinrb.mass(df, window: 20)
Yfinrb.max(df, column: 'Adj Close', window: 20)
Yfinrb.md(df, column: 'Adj Close', window: 20)
Yfinrb.median_price(df)
Yfinrb.mfi(df, window: 20)
Yfinrb.min(df, column: 'Adj Close', window: 20)
Yfinrb.mom(df, column: 'Adj Close', window: 5)
Yfinrb.moving_avgs(df, window: 20)
Yfinrb.natr(df, window: 20)
Yfinrb.nvi(df)
Yfinrb.obv(df)
Yfinrb.ppo(df, column: 'Adj Close', short_window: 12, long_window: 26)
Yfinrb.psar(df, acceleration_factor_step: 0.2, acceleration_factor_maximum: 2)
Yfinrb.pvi(df)
Yfinrb.qstick(df, window: 20)
Yfinrb.roc(df, column: 'Adj Close', window: 20)
Yfinrb.rocr(df, column: 'Adj Close', window: 20)
Yfinrb.rsi(df, window: 20)
Yfinrb.sma(df, column: 'Adj Close', window: 20)
Yfinrb.stddev(df, column: 'Adj Close', window: 20)
Yfinrb.stderr(df, column: 'Adj Close', window: 20)
Yfinrb.stochrsi(df, column: 'Adj Close', window: 20)
Yfinrb.sum(df, column: 'Adj Close', window: 20)
Yfinrb.tema(df, column: 'Adj Close', window: 20)
Yfinrb.tr(df, column: 'Adj Close')
Yfinrb.trima(df, column: 'Adj Close', window: 20)
Yfinrb.trix(df, column: 'Adj Close', window: 20)
Yfinrb.trima(df, column: 'Adj Close', window: 20)
Yfinrb.tsf(df, column: 'Adj Close', window: 20)
Yfinrb.typical_price(df)
Yfinrb.ultosc(df, short_window: 5, medium_window: 12, long_window: 26)
Yfinrb.weighted_close_price(df)
Yfinrb.var(df, column: 'Adj Close', window: 20)
Yfinrb.vhf(df, column: 'Adj Close', window: 20)
Yfinrb.vidya(df, column: 'Adj Close', short_window: 5, long_window: 20, alpha: 0.2)
Yfinrb.volatility(df, column: 'Adj Close', window: 20)
Yfinrb.vosc(df, column: 'Adj Close', short_window: 5, long_window: 20)
Yfinrb.vol_weighted_moving_avg(df, window: 20)
Yfinrb.wad(df)
Yfinrb.wcprice(df)
Yfinrb.wilders(df, column: 'Adj Close', window: 20)
Yfinrb.willr(df, window: 20)
Yfinrb.wma(df, column: 'Adj Close', window: 5)
Yfinrb.zlema(df, column: 'Adj Close', window: 5)
```


---

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'yfinrb'
```

And then execute:

    $ bundle install

Or install it yourself as:

    $ gem install yfinrb

---

## Development

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and the created tag, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/bmck/yfinrb.

---

### Legal Stuff

The **yfinrb** gem is available as open source under the **MIT Software License** (https://opensource.org/licenses/MIT). See
the [LICENSE.txt](./LICENSE.txt) file in the release for details.


AGAIN - yfinrb is **not** affiliated, endorsed, or vetted by Yahoo, Inc. It's
an open-source tool that uses Yahoo's publicly available APIs, and is
intended for research and educational purposes. You should refer to Yahoo!'s terms of use
([here](https://policies.yahoo.com/us/en/yahoo/terms/product-atos/apiforydn/index.htm),
[here](https://legal.yahoo.com/us/en/yahoo/terms/otos/index.html), and
[here](https://policies.yahoo.com/us/en/yahoo/terms/index.htm)) for
details on your rights to use the actual data downloaded.

---