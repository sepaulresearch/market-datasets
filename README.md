<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/sepaulresearch/.github/main/profile/assets/sepaul_research_logo_dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/sepaulresearch/.github/main/profile/assets/sepaul_research_logo.svg">
    <img alt="Sepaul Research" src="https://raw.githubusercontent.com/sepaulresearch/.github/main/profile/assets/sepaul_research_logo.svg" width="460">
  </picture>
</p>

<h1 align="center">Empirical Market Datasets</h1>

<p align="center">
  <strong>Department of Quantitative Research &amp; Modeling</strong><br>
  <em>Multi-Timeframe Historical OHLCV Series &bull; Raw-Spread ECN/STP OTC Execution Feeds</em>
</p>

<p align="center">
  <a href="https://sepaulresearch.com"><img src="https://img.shields.io/badge/Official%20Portal-sepaulresearch.com-0066cc?style=for-the-badge&logo=googlechrome&logoColor=white" alt="Sepaul Research Portal"></a>
  <a href="https://orcid.org/0009-0005-9817-2488"><img src="https://img.shields.io/badge/ORCID-0009--0005--9817--2488-a6ce39?style=for-the-badge&logo=orcid&logoColor=white" alt="ORCID"></a>
  <a href="./LICENSE"><img src="https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey?style=for-the-badge" alt="License: CC BY 4.0"></a>
</p>

---

## Overview

This repository hosts curated, broker-anonymized historical OHLCV market datasets maintained by **Sepaul Research** for reproducible empirical finance research, non-linear dynamical systems analysis, statistical mechanics modeling, and execution-microstructure auditing.

All time series are captured from a **Raw-Spread ECN/STP OTC Feed (Zero-Spread / Commission-Based Tier)** and normalized into consistent, UTF-8 CSV schemas indexed in **UTC**.

---

## Dataset Inventory

### Packaged Files

| Path | Instrument | Format | Size | Scope |
| :--- | :--- | :--- | ---: | :--- |
| [`data/xauusd/xauusd_raw_spread_multitf.zip`](data/xauusd/xauusd_raw_spread_multitf.zip) | **XAU/USD** (Spot Gold CFD) | `.zip` (6 CSVs) | 46.18 MiB | Multi-timeframe archive (`M1`, `M5`, `M10`, `M15`, `M30`, `H1`) |
| [`data/xauusd/xauusd_raw_spread_h1.csv`](data/xauusd/xauusd_raw_spread_h1.csv) | **XAU/USD** (Spot Gold CFD) | `.csv` (raw) | 2.10 MiB | Uncompressed `H1` series for direct streaming/ingestion |
| [`data/btcusd/btcusd_raw_spread_m1.zip`](data/btcusd/btcusd_raw_spread_m1.zip) | **BTC/USD** (Bitcoin CFD) | `.zip` (1 CSV) | 1.65 MiB | High-frequency `M1` historical series |

### Verified Time-Series Coverage

| Instrument | Internal File | Timeframe | Bars (Rows) | Start Timestamp (UTC) | End Timestamp (UTC) |
| :--- | :--- | :---: | ---: | :--- | :--- |
| **XAU/USD** | `xauusd_m1.csv` | `M1` | 2,032,041 | `2021-01-03 23:05:00` | `2026-10-01 14:56:00` |
| **XAU/USD** | `xauusd_m5.csv` | `M5` | 407,232 | `2021-01-03 23:05:00` | `2026-10-01 14:55:00` |
| **XAU/USD** | `xauusd_m10.csv` | `M10` | 203,772 | `2021-01-03 23:00:00` | `2026-10-01 14:50:00` |
| **XAU/USD** | `xauusd_m15.csv` | `M15` | 135,847 | `2021-01-03 23:00:00` | `2026-10-01 14:45:00` |
| **XAU/USD** | `xauusd_m30.csv` | `M30` | 67,943 | `2021-01-03 23:00:00` | `2026-10-01 14:30:00` |
| **XAU/USD** | `xauusd_h1.csv` | `H1` | 33,991 | `2021-01-03 23:00:00` | `2026-10-01 14:00:00` |
| **BTC/USD** | `btcusd_m1.csv` | `M1` | 99,000 | `2026-07-19 10:32:00` | `2026-09-26 05:04:00` |

> **Note:** `data/xauusd/xauusd_raw_spread_h1.csv` is byte-equivalent (LF-normalized) to `xauusd_h1.csv` inside `xauusd_raw_spread_multitf.zip`.

---

## Feed Classification & Methodological Notes

- **Venue & Feed Typology:** **Raw-Spread ECN/STP OTC Feed (Zero-Spread / Commission-Based Tier)**. Quotes originate from an aggregated over-the-counter (OTC) CFD liquidity stream rather than a centralized exchange order book (CLOB).
- **Timestamp Standard:** Bar-opening timestamps formatted as `YYYY-MM-DD HH:MM:SS` in **UTC**.
- **Quoted Spread Regime:** Because the underlying account tier operates on a commission-based raw-spread structure, quoted top-of-book spreads are frequently zero during liquid sessions (e.g., `88.2%` of `XAU/USD H1` bars exhibit `spread = 0`, with a 95th percentile of `50` points and tail widening up to `847` points during rollover/news shocks; `100.0%` of `BTC/USD M1` bars exhibit `spread = 0`). **Researchers modeling net PnL or execution friction must explicitly add per-lot round-turn commissions and realistic slippage**, as `spread = 0` does not imply zero transaction cost.
- **Volume Semantics:** As is standard on OTC/CFD ECN feeds, centralized exchange trade volume is unavailable (`real_volume` is identically `0` across all series). Market activity is proxied by `tick_volume` (number of discrete price-quote updates arriving within each bar).

---

## Schema & Data Dictionary

All CSV files share an identical 8-column header:

```text
time,open,high,low,close,tick_volume,spread,real_volume
```

| Column | Type | Unit / Semantics |
| :--- | :--- | :--- |
| `time` | `datetime` (ISO-like) | Bar open timestamp in UTC (`YYYY-MM-DD HH:MM:SS`). |
| `open` | `float64` | Bar opening bid price (quote currency: USD). |
| `high` | `float64` | Bar highest bid price (quote currency: USD). |
| `low` | `float64` | Bar lowest bid price (quote currency: USD). |
| `close` | `float64` | Bar closing bid price (quote currency: USD). |
| `tick_volume` | `int64` | Count of discrete quote updates (ticks) observed during the bar. |
| `spread` | `int64` | Minimum/recorded bar bid-ask spread in minimal price increments (points). |
| `real_volume` | `int64` | Exchange-reported traded volume (`0` on OTC/CFD feeds; retained for schema compatibility). |

---

## Direct Access & Integrity Verification

### Raw Download (`curl`)

```bash
# XAU/USD multi-timeframe archive (M1, M5, M10, M15, M30, H1)
curl -L -O https://raw.githubusercontent.com/sepaulresearch/market-datasets/main/data/xauusd/xauusd_raw_spread_multitf.zip

# XAU/USD uncompressed H1 CSV
curl -L -O https://raw.githubusercontent.com/sepaulresearch/market-datasets/main/data/xauusd/xauusd_raw_spread_h1.csv

# BTC/USD M1 archive
curl -L -O https://raw.githubusercontent.com/sepaulresearch/market-datasets/main/data/btcusd/btcusd_raw_spread_m1.zip
```

### Quick Ingestion (Python / `pandas`)

```python
import pandas as pd

url = "https://raw.githubusercontent.com/sepaulresearch/market-datasets/main/data/xauusd/xauusd_raw_spread_h1.csv"
df = pd.read_csv(url, parse_dates=["time"])
print(df.info())
```

### Cryptographic Verification (SHA-256)

Every released artifact is anchored in [`CHECKSUMS.sha256`](CHECKSUMS.sha256) for reproducible auditing:

```bash
sha256sum -c CHECKSUMS.sha256
```

---

## Citation

If you use these datasets in academic publications, working papers, or empirical benchmarks, please cite this repository using the metadata in [`CITATION.cff`](CITATION.cff) or the BibTeX entry below:

```bibtex
@dataset{sepaul_research_market_datasets_2026,
  author       = {Sepaul, Mikael Bradford S. R.},
  title        = {Sepaul Research Empirical Market Datasets (Raw-Spread ECN/STP OTC Feeds)},
  year         = {2026},
  version      = {1.0.0},
  publisher    = {Sepaul Research},
  url          = {https://github.com/sepaulresearch/market-datasets}
}
```

---

## License

This dataset collection is released under the **Creative Commons Attribution 4.0 International (CC BY 4.0)** license. See [`LICENSE`](LICENSE) for full terms.

---

<p align="center">
  <sub>&copy; 2026 Sepaul Research &middot; Department of Quantitative Research &amp; Modeling &middot; Brasília, DF, Brazil</sub>
</p>
