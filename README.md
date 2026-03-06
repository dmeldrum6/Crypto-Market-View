# Crypto-Market-View

A single-file, zero-dependency cryptocurrency market dashboard built with plain HTML, CSS, and vanilla JavaScript. Data is sourced from the free [n.exchange public API](https://nexchange.io/api/v1/) — no API key required.

---

## Screenshot

> Open `index.html` directly in any modern browser — no build step, no server required.

---

## Features

- **Live market data** — prices, 24h high/low/volume, bid/ask spread, pulled from the n.exchange API
- **Auto-refresh** — all ticker data refreshes every 15 seconds with a visible countdown timer
- **Spotlight panel** — click any pair to see a large price display, full stats row, and a 24h area chart
- **Market grid** — responsive card grid (3–4 columns) sorted by 24h volume, with per-card mini sparklines
- **Price flash animations** — cards briefly highlight green or red whenever a price changes on refresh
- **Smooth SVG charts** — all sparklines are inline SVG drawn with cubic Bézier curves and a glow filter; no canvas, no charting libraries
- **Dark terminal aesthetic** — CSS-only scanline texture, neon-green accents, monospace font throughout
- **Graceful error handling** — failed ticker fetches show an `ERR` badge on the affected card; a fatal load failure keeps the loading screen visible with a descriptive message
- **Parallel data fetching** — `Promise.all()` is used to fetch all tickers and OHLCV histories simultaneously on load

---

## Tech Stack

| Concern | Implementation |
|---|---|
| Language | Vanilla JavaScript (ES2020, `async/await`) |
| Styling | Pure CSS — no frameworks |
| Charts | Inline SVG with cubic Bézier paths |
| Data | [n.exchange REST API](https://nexchange.io/api/v1/) |
| Dependencies | **None** — zero npm, zero CDN imports |
| Build step | **None** — open `index.html` directly |

---

## API Endpoints Used

| Endpoint | Purpose |
|---|---|
| `GET /market/` | Discover all available trading pairs |
| `GET /market/{pair}/ticker/` | Live price data (last, bid, ask, high, low, volume) |
| `GET /market/{pair}/ohlcv/?interval=1h` | Candlestick history for sparkline charts |

---

## Layout

```
┌─────────────────────────────────────────────────────────────┐
│  NEXTERM          [LIVE]    HH:MM:SS    LAST UPDATE: ...     │  ← Header
├─────────────────────────────────────────────────────────────┤
│  PAIRS:  [BTC/ETH]  [BTC/USDT]  [ETH/USDT]  [LTC/BTC] ...  │  ← Pair selector
├──────────────────────────┬──────────────────────────────────┤
│  BTC / USDT              │  24H PRICE CHART                 │
│  84,231.0000             │  ╭──╮                            │  ← Spotlight
│  ▲ +2.41%                │ ╭╯  ╰─╮      ╭─╮                │    panel
│  HIGH  LOW  VOL          │╭╯      ╰──────╯ ╰───────●        │
│  BID   ASK               │                                  │
├──────────────────────────┴──────────────────────────────────┤
│  MARKET OVERVIEW                                12 PAIRS     │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐        │
│  │ BTC/USDT │ │ ETH/USDT │ │ BTC/ETH  │ │ LTC/BTC  │        │  ← Market
│  │ 84,231   │ │ 3,201    │ │ 0.03802  │ │ 0.001621 │        │    grid
│  │ ▲ +2.41% │ │ ▼ -0.8%  │ │ ▲ +1.2%  │ │ ▼ -0.3%  │        │
│  │ ~~~chart~│ │ ~~~chart~│ │ ~~~chart~│ │ ~~~chart~│        │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘        │
├─────────────────────────────────────────────────────────────┤
│  Powered by n.exchange API              REFRESHING IN 12s   │  ← Footer
└─────────────────────────────────────────────────────────────┘
```

---

## Usage

1. Clone the repository:
   ```bash
   git clone https://github.com/dmeldrum6/Crypto-Market-View.git
   cd Crypto-Market-View
   ```

2. Open the dashboard:
   ```bash
   # Any of these will work:
   open index.html           # macOS
   xdg-open index.html       # Linux
   start index.html          # Windows
   ```
   Or simply drag `index.html` into your browser.

> **Note:** The n.exchange API must be reachable from your browser. If you see an error on load, check your network connection or browser console for CORS/network details.

---

## Configuration

A small `CFG` object at the top of the `<script>` block exposes the key settings:

```js
const CFG = {
  API:          'https://nexchange.io/api/v1',
  MAX_PAIRS:    12,       // how many pairs to track
  REFRESH_SECS: 15,       // ticker auto-refresh interval (seconds)
  OHLCV_INTV:   '1h',    // OHLCV candle interval
  OHLCV_PTS:    24,       // number of candles used for sparklines
  PREFERRED: [ ... ],     // ordered list of pairs to prioritise
};
```

---

## License

See [LICENSE](LICENSE).
