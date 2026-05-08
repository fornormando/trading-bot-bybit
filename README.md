# 🤖 Automated BTC/USDT Trading Bot — Bybit

> Fully automated spot trading bot for Bybit using a multi-indicator confluence strategy. Built in Python with real-time signal detection, dynamic position sizing, and autonomous order execution.

---

## 📌 Overview

This bot connects to the Bybit exchange via REST API and continuously monitors BTC/USDT on a 5-minute timeframe. It only enters a trade when **all five technical indicators align simultaneously**, reducing false signals and increasing trade quality.

---

## ⚙️ Strategy Logic

### ✅ Entry Signals (ALL must be true)
| Indicator | Condition |
|---|---|
| **Didi Index** | MA3 crosses above MA8 (bullish crossover) or MA3 > MA8 > MA20 |
| **DMI / ADX** | ADX ≥ 25 and +DI > -DI (strong uptrend confirmed) |
| **Bollinger Bands** | Bands expanding (increasing volatility/momentum) |

### 🚪 Exit Signals (ALL must be true)
| Indicator | Condition |
|---|---|
| **DMI Kick** | Trend reversal detected (+DI / -DI flip or ADX < 20) |
| **Bollinger Bands** | Bands contracting (momentum fading) |
| **Stochastic** | %K and %D both above 80 (overbought) |
| **MACD** | MACD line below signal line (bearish crossover) |

---

## 🧠 Technical Indicators

- **Didi Index** — Triple moving average system (MA3, MA8, MA20)
- **DMI / ADX** — Directional Movement Index with trend strength filter
- **Bollinger Bands** — Volatility bands (20-period, 2 std deviations)
- **Stochastic Oscillator** — Momentum oscillator (14, 3 periods)
- **MACD** — Trend-following momentum (12, 26, 9)

---

## 🏗️ Architecture

```
BybitTradingBot
│
├── _initialize_exchange()       # CCXT Bybit connection + sandbox mode
├── get_balance()                # Direct REST API call to FUND account
│
├── Indicators
│   ├── calculate_didi_index()
│   ├── calculate_dmi()
│   ├── calculate_bollinger_bands()
│   ├── calculate_stochastic()
│   └── calculate_macd()
│
├── Entry Signals
│   ├── check_didi_buy_signal()
│   ├── check_dmi_trend()
│   ├── check_bollinger_opening()
│   └── check_entry_signals()    # AND logic — all must be true
│
├── Exit Signals
│   ├── check_dmi_kick()
│   ├── check_bollinger_closing()
│   ├── check_stochastic_overbought()
│   ├── check_macd_rejection()
│   └── check_exit_signals()     # AND logic — all must be true
│
├── Execution
│   ├── calculate_position_size()
│   ├── execute_buy()
│   └── execute_sell()
│
└── run()                        # Main loop (default: 60s interval)
```

---

## 🚀 Getting Started

### Prerequisites

```bash
pip install ccxt pandas numpy requests
```

### Configuration

```python
API_KEY    = "YOUR_API_KEY_HERE"
API_SECRET = "YOUR_API_SECRET_HERE"
```

### Run

```bash
python bot_trading_final.py
```

> Set `testnet=True` for paper trading on Bybit Testnet before going live.

---

## 🔐 Security Notes

- **Never commit real API keys** to version control
- Use environment variables or a `.env` file in production:

```python
import os
API_KEY    = os.getenv("BYBIT_API_KEY")
API_SECRET = os.getenv("BYBIT_API_SECRET")
```

- Restrict API key permissions to **Trade only** (no withdrawal access)

---

## 📊 Configuration Parameters

| Parameter | Default | Description |
|---|---|---|
| `symbol` | `BTC/USDT` | Trading pair |
| `primary_timeframe` | `5m` | Candle timeframe |
| `max_investment` | `$1,000` | Max USDT per trade |
| `testnet` | `True` | Use Bybit Testnet |
| `check_interval` | `60s` | Loop frequency |

---

## 📁 Files

```
├── bot_trading_final.py   # Main bot
└── trading_bot.log        # Auto-generated trade log
```

---

## ⚠️ Disclaimer

This project is for **educational purposes only**. Cryptocurrency trading involves significant financial risk. Always test thoroughly on testnet before using real funds. Past performance does not guarantee future results.

---

## 🛠️ Built With

![Python](https://img.shields.io/badge/Python-3.8+-blue?logo=python)
![CCXT](https://img.shields.io/badge/CCXT-Latest-green)
![Bybit](https://img.shields.io/badge/Exchange-Bybit-yellow)
