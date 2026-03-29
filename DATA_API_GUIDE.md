# 真实宏观数据 API 使用指南

## 数据源说明

### 1. yfinance（推荐）

**安装：**
```bash
pip install yfinance
```

**可用数据：**
- **美元指数**: DX-Y.NYB
- **10 年期美债收益率**: ^TNX（TIPS 近似）
- **COMEX 黄金**: GC=F
- **VIX 指数**: ^VIX
- **黄金 ETF**: GLD（持仓近似）

**优点：**
- 免费、稳定
- 数据质量高
- 更新及时

**缺点：**
- 部分宏观数据缺失
- 需要网络访问

### 2. akshare（备用）

**安装：**
```bash
pip install akshare
```

**可用数据：**
- **518880 ETF**: fund_etf_hist_em
- **美国 CPI**: macros_usa_cpi_monthly
- **PMI**: macros_global_pmi
- **黄金持仓**: gold_etf_holdings

**优点：**
- 国内数据全面
- 中文文档

**缺点：**
- API 经常变化
- 网络不稳定

### 3. 模拟数据（备用）

当真实数据不可用时，自动使用基于历史统计的模拟数据。

## 使用方法

### 快速开始

```python
from data.real_data_api import build_macro_factors

# 获取宏观因子数据
factors = build_macro_factors("2021-01-01", "2025-12-31")

# 查看数据
print(factors.head())
print(factors.columns)
```

### 获取单个数据

```python
from data.real_data_api import (
    fetch_dxy_yf,
    fetch_tips_yf,
    fetch_gold_yf,
    fetch_vix_yf,
    fetch_518880_ak
)

# 美元指数
dxy = fetch_dxy_yf("2021-01-01", "2025-12-31")

# 黄金价格
gold = fetch_gold_yf("2021-01-01", "2025-12-31")

# VIX
vix = fetch_vix_yf("2021-01-01", "2025-12-31")

# 518880
price = fetch_518880_ak("20210101", "20251231")
```

### 缓存管理

数据自动缓存到 `data/cache/` 目录，默认缓存 24 小时。

```python
from data.real_data_api import load_cache, save_cache

# 手动加载缓存
cached = load_cache("dxy_2021_2025", max_age_hours=48)

# 保存缓存
save_cache(dataframe, "my_data")
```

## 数据字段说明

### 宏观因子数据 (macro_factors_real.csv)

| 字段 | 说明 | 单位 |
|------|------|------|
| date | 日期 | - |
| tips_current | TIPS 实际利率（当前） | % |
| tips_previous | TIPS 实际利率（上期） | % |
| dxy_current | 美元指数（当前） | - |
| dxy_previous | 美元指数（上期） | - |
| vix_current | VIX 指数（当前） | - |
| vix_previous | VIX 指数（上期） | - |
| etf_holdings_current | ETF 持仓（当前） | 吨 |
| etf_holdings_previous | ETF 持仓（上期） | 吨 |
| cpi_current | CPI（当前） | % |
| cpi_previous | CPI（上期） | % |
| central_bank_buying | 央行购金 | 吨/月 |
| comex_net_long | COMEX 净多头 | 比例 |
| oil_gold_ratio_current | 原油/黄金比价 | - |
| us_eu_spread_current | 美欧利差 | % |
| fiscal_deficit_current | 财政赤字率 | % |
| pmi_actual | PMI 实际值 | - |
| pmi_expected | PMI 预期值 | - |
| treasury_spread_current | 美债期限利差 | % |

## 回测使用

```python
import pandas as pd
from data.real_data_api import build_macro_factors
from strategies.gold_trend_factor import GoldTrendFactorStrategy

# 1. 获取宏观数据
macro = build_macro_factors("2021-01-01", "2025-12-31")

# 2. 获取价格数据
price = pd.read_csv("backtest/518880_final_positions.csv", parse_dates=['date'])

# 3. 运行回测
strategy = GoldTrendFactorStrategy()

for idx, row in macro.iterrows():
    market_data = row.to_dict()
    
    signal = strategy.analyze(
        current_price=price['close'].iloc[idx],
        ma50=price['ma50'].iloc[idx],
        ma200=price['ma200'].iloc[idx],
        monthly_close=price['monthly_close'].iloc[idx],
        ma12=price['ma12'].iloc[idx],
        market_data=market_data
    )
    
    print(f"{row['date']}: 得分={signal.total_score}, 仓位={signal.position_ratio}")
```

## 常见问题

### Q: 数据获取失败怎么办？
A: 系统会自动使用模拟数据作为备用。检查网络连接或安装 yfinance。

### Q: 如何更新缓存？
A: 删除 `data/cache/` 目录下的文件，或等待 24 小时自动过期。

### Q: 数据不准确？
A: 
1. TIPS 使用 10 年期国债收益率近似
2. ETF 持仓使用 GLD 价格反推
3. 建议接入专业数据源（Bloomberg、Wind）

### Q: 如何提高数据质量？
A: 
1. 安装 yfinance（优先使用）
2. 安装 akshare（国内数据）
3. 接入付费数据源（FRED、WGC、CFTC）

## 专业数据源（推荐）

### 1. 美联储 FRED
- **网址**: https://fred.stlouisfed.org
- **数据**: TIPS、国债收益率、CPI
- **费用**: 免费
- **API**: https://fred.stlouisfed.org/docs/api/

### 2. 世界黄金协会
- **网址**: https://www.gold.org
- **数据**: ETF 持仓、央行购金
- **费用**: 免费

### 3. CFTC COT
- **网址**: https://www.cftc.gov
- **数据**: COMEX 持仓
- **费用**: 免费

### 4. Bloomberg/Wind
- **数据**: 全面的宏观和金融数据
- **费用**: 付费（昂贵）

## 文件结构

```
guojin_auto_trading/
├── data/
│   ├── real_data_api.py       # 数据获取模块
│   ├── real_macro_data.py     # 旧版（保留）
│   ├── macro_data_simple.py   # 简化版（保留）
│   ├── cache/                 # 数据缓存
│   └── macro_factors_real.csv # 输出数据
└── backtest/
    └── backtest_final.py      # 回测脚本
```

## 下一步

1. **安装依赖**: `pip install yfinance akshare`
2. **获取数据**: `python data/real_data_api.py`
3. **运行回测**: `python backtest/backtest_final.py`
4. **查看报告**: `backtest/518880_final_report.md`

---

*文档版本：V1.0.0*
*更新时间：2026-03-22*
