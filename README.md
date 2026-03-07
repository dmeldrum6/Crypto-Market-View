# Crypto-Market-View

A single-file, zero-dependency cryptocurrency market dashboard built with plain HTML, CSS, and vanilla JavaScript. Data is sourced from the free [CoinGecko public API](https://api.coingecko.com/api/v3/) — no API key required.

---

## Screenshot

<img width="1898" height="863" alt="image" src="https://github.com/user-attachments/assets/d685f2a1-9c7b-49fa-87f3-8d70153e6bbd" />

---

## Features

- **Live market data** — prices, 24h high/low/volume, and % change pulled from the CoinGecko API
- **Auto-refresh** — all ticker data refreshes every 60 seconds with a visible countdown timer
- **Spotlight panel** — click any coin to see a large price display, full stats row, and a 24h area chart
- **Market grid** — responsive card grid sorted by 24h volume, with per-card mini sparklines
- **Price flash animations** — cards briefly highlight green or red whenever a price changes on refresh
- **Smooth SVG charts** — all sparklines are inline SVG drawn with cubic Bézier curves and a glow filter; no canvas, no charting libraries
- **Dark terminal aesthetic** — CSS-only scanline texture, neon-green accents, monospace font throughout
- **Graceful error handling** — failed ticker fetches show an `ERR` badge on the affected card; a fatal load failure keeps the loading screen visible with a descriptive message
- **Progressive sparkline loading** — OHLCV chart data is fetched in the background after the UI is visible, staggered to respect free-tier rate limits; sparklines appear coin by coin without blocking the initial render

---

## Tech Stack

| Concern | Implementation |
|---|---|
| Language | Vanilla JavaScript (ES2020, `async/await`) |
| Styling | Pure CSS — no frameworks |
| Charts | Inline SVG with cubic Bézier paths |
| Data | [CoinGecko REST API](https://api.coingecko.com/api/v3/) (free, no auth) |
| Dependencies | **None** — zero npm, zero CDN imports |
| Build step | **None** — open `index.html` directly |

---

## API Endpoints Used

| Endpoint | Purpose |
|---|---|
| `GET /coins/markets?vs_currency=usd&ids=...` | Batch-fetch prices, 24h high/low/volume, and % change for all tracked coins in a single request |
| `GET /coins/{id}/ohlc?vs_currency=usd&days=1` | 30-min candlestick history (~48 points) for sparkline charts |

---

## Layout

```
┌─────────────────────────────────────────────────────────────┐
│  NEXTERM          [LIVE]    HH:MM:SS    LAST UPDATE: ...     │  ← Header
├─────────────────────────────────────────────────────────────┤
│  PAIRS:  [BTC/USD]  [ETH/USD]  [SOL/USD]  [XRP/USD] ...     │  ← Pair selector
├──────────────────────────┬──────────────────────────────────┤
│  BTC / USD               │  24H PRICE CHART                 │
│  84,231.0000             │  ╭──╮                            │  ← Spotlight
│  ▲ +2.41%                │ ╭╯  ╰─╮      ╭─╮                │    panel
│  HIGH  LOW  VOL          │╭╯      ╰──────╯ ╰───────●        │
│  BID   ASK               │                                  │
├──────────────────────────┴──────────────────────────────────┤
│  MARKET OVERVIEW                                12 PAIRS     │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐        │
│  │ BTC/USD  │ │ ETH/USD  │ │ BNB/USD  │ │ SOL/USD  │        │  ← Market
│  │ 84,231   │ │ 3,201    │ │ 612.40   │ │ 142.80   │        │    grid
│  │ ▲ +2.41% │ │ ▼ -0.8%  │ │ ▲ +1.2%  │ │ ▼ -0.3%  │        │
│  │ ~~~chart~│ │ ~~~chart~│ │ ~~~chart~│ │ ~~~chart~│        │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘        │
├─────────────────────────────────────────────────────────────┤
│  Powered by CoinGecko                   REFRESHING IN 58s   │  ← Footer
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

> **Note:** The CoinGecko free API allows roughly 30 requests per minute. The app is designed to stay well within this limit: the initial load makes one batch ticker request, then fetches sparkline data one coin at a time with a 250 ms gap between each call.

---

## Configuration

A `CFG` object and a `PAIRS` constant at the top of the `<script>` block expose the key settings:

```js
const CFG = {
  API:          'https://api.coingecko.com/api/v3',
  MAX_PAIRS:    12,   // how many coins to track (max = length of PAIRS)
  REFRESH_SECS: 60,   // ticker auto-refresh interval (seconds)
  OHLCV_PTS:    24,   // number of candles used for sparklines
};

/* Edit this list to add, remove, or reorder tracked coins */
const PAIRS = [
  { pair: 'BTCUSD',  base: 'BTC',  quote: 'USD', coinId: 'bitcoin'   },
  { pair: 'ETHUSD',  base: 'ETH',  quote: 'USD', coinId: 'ethereum'  },
  // ... one entry per coin
];
```

To add a coin, append an entry to `PAIRS` using its [CoinGecko coin ID](https://api.coingecko.com/api/v3/coins/list).

---

## License

See [LICENSE](LICENSE).
