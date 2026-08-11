# Swiss National Bank Exchange Rate API client

Official **Swiss National Bank** (Switzerland) monthly exchange rates in Node.js / TypeScript — 26 currencies against the CHF, with history back to 1914. Zero dependencies, works in Node 18+, Bun, Deno, and edge runtimes (uses global `fetch`).

These are the *published central bank rates* required for tax filings, customs valuations, audits, and compliant invoicing — not moving market rates. Every response carries the publisher's own publication date.

Powered by [AllRatesToday](https://allratestoday.com/central-bank-rates-api/snb/). Get a free API key at [allratestoday.com/register](https://allratestoday.com/register) — no credit card required.

## Install

```bash
npm install snb-exchange-rate
```

## Quick start

```js
import { getRate, getLatestRates } from 'snb-exchange-rate';

// One pair at the official Swiss National Bank rate
const pair = await getRate('EUR', 'CHF', { apiKey: 'art_live_...' });
console.log(pair.rate, pair.rate_date); // e.g. EUR -> CHF on the bank's own date

// The bank's full published table
const table = await getLatestRates({ apiKey: 'art_live_...' });
console.log(table.rate_date, table.rates.length);
```

## Historical data (paid plans)

```js
import { getRatesForDate, getHistory } from 'snb-exchange-rate';

// The official table for an invoice date — weekends/holidays return the
// most recent published date, flagged via published_on_requested_date.
const day = await getRatesForDate('2026-06-30', { apiKey: 'art_live_...' });

// Daily series for one pair
const series = await getHistory(
  { source: 'EUR', target: 'CHF', from: '2026-01-01' },
  { apiKey: 'art_live_...' }
);
```

## Currencies covered

Swiss National Bank currently publishes rates covering **27 currencies** (as of the latest table):

`ARS` · `AUD` · `BRL` · `CAD` · `CHF` · `CNY` · `CZK` · `DKK` · `EUR` · `GBP` · `HKD` · `HUF` · `JPY` · `KRW` · `MXN` · `MYR` · `NOK` · `NZD` · `PLN` · `RUB` · `SEK` · `SGD` · `THB` · `TRY` · `USD` · `XDR` · `ZAR`

Pairs the central bank does not print directly are resolved from this table (see below).

## Published vs derived rates

If Swiss National Bank does not print a pair directly, the API resolves it from the bank's table (inverse, or a cross rate via CHF) and flags it `derived: true` with the `method` — so official and computed values are never confused.

## Notes

- Every request counts toward your AllRatesToday monthly quota. Rates change once per business day — cache a day's table locally and a small quota goes a long way.
- Latest rates are on every plan (including free); historical dates and time series need a [paid plan](https://allratestoday.com/pricing/).
- Full API reference: [allratestoday.com/docs#central-bank](https://allratestoday.com/docs/#central-bank) · All covered sources: [central bank rates API](https://allratestoday.com/central-bank-rates-api/)

## License

MIT
