# 日线级别回测系统

## 概述

本系统支持基于日线数据的策略回测，包括：

1. **数据获取**：通过 mx-data 或本地文件获取日线数据
2. **策略框架**：RSI 四阶段、均线趋势、乖离率等多种策略
3. **回测引擎**：完整的交易模拟和绩效计算
4. **报告生成**：详细的回测报告和交易记录

## 快速开始

### 1. 运行回测

```bash
cd guojin_auto_trading
python backtest/backtest_daily_2023_2026.py
```

### 2. 查看结果

回测完成后，在 `backtest/output/` 目录查看：

- `518880_daily_trades.csv` - 交易记录
- `518880_daily_records.csv` - 每日资产记录
- `518880_daily_backtest_report.md` - 回测报告

## 当前状态

### ✅ 已完成

- [x] 日线回测引擎
- [x] 多策略支持（RSI、均线、乖离率）
- [x] 技术指标计算（RSI、MACD、ADX、均线）
- [x] 交易执行模拟（含手续费、滑点、印花税）
- [x] 绩效指标计算（收益率、夏普比率、最大回撤、胜率）
- [x] 缓存机制
- [x] 报告生成

### ⚠️ 数据源说明

**当前使用的是月度数据**（108 条，2023-2026），原因：

1. mx-data API 需要配置 HTTP 端点才能自动调用
2. 本地只有月度回测数据（`518880_github_strategy.csv`）

**要获取真正的日线数据**，有以下方案：

### 方案 1：配置 mx-data HTTP API（推荐）

在 `openclaw.json` 中添加：

```json
{
  "skills": {
    "mx-data": {
      "http_enabled": true,
      "endpoint": "http://localhost:18789/api/skill/mx-data"
    }
  }
}
```

然后查询日线数据：

```python
from data.mx_data_client import MXDataClient

client = MXDataClient()
result = client.query("518880 日线数据 2023-01-01 至 2026-03-24")
```

### 方案 2：从 QMT 获取日线数据

如果你有国金 QMT 交易终端，可以使用 `xtquant` 获取日线：

```python
from xtquant import xtdata

# 下载日线数据
xtdata.download_history_data('518880.SH', period='1d', start_date='20230101', end_date='20260324')

# 获取日线数据
data = xtdata.get_market_data('518880.SH', period='1d')
```

### 方案 3：使用模拟数据

系统会自动生成模拟日线数据用于测试：

```python
fetcher = DailyDataFetcher('518880')
df = fetcher._generate_simulated_daily('2023-01-01', '2026-03-24')
```

## 策略说明

### RSI 四阶段策略

基于 RSI 指标的超买超卖信号：

- **买入**：RSI6 < 30 且 RSI6 < RSI12（超卖）
- **卖出**：RSI6 > 70 且 RSI6 > RSI12（超买）
- **过滤**：MA55 趋势、MACD 确认、ADX 趋势强度

### 均线趋势策略

基于均线多头/空头排列：

- **多头**：价格 > MA20 > MA60
- **空头**：价格 < MA20 < MA60

### 乖离率策略

基于价格偏离均线的程度：

- **买入**：乖离率 < -5%（超跌）
- **卖出**：乖离率 > 15%（超涨）

## 配置参数

在 `backtest_daily_2023_2026.py` 中修改 `CONFIG`：

```python
CONFIG = {
    'symbol': '518880',              # 交易标的
    'start_date': '2023-01-01',      # 回测开始日期
    'end_date': '2026-03-24',        # 回测结束日期
    'initial_capital': 100000.0,     # 初始资金
    'commission_rate': 0.0003,       # 手续费率（万三）
    'stamp_tax_rate': 0.001,         # 印花税（0.1%）
    'slippage': 0.001,               # 滑点（0.1%）
    
    # RSI 策略参数
    'rsi_short_period': 6,
    'rsi_long_period': 12,
    'oversold_threshold': 30,
    'overbought_threshold': 70,
    
    # 仓位管理
    'base_position': 0.5,
    'max_position': 1.0,
    'min_position': 0.2,
}
```

## 绩效指标说明

| 指标 | 说明 | 计算方式 |
|------|------|----------|
| 总收益率 | 回测期间总收益百分比 | (最终资产 - 初始资金) / 初始资金 |
| 年化收益率 | 年化后的收益率 | (1 + 总收益率)^(1/年数) - 1 |
| 最大回撤 | 最大资产下降幅度 | (峰值 - 谷值) / 峰值 |
| 夏普比率 | 风险调整后收益 | 年化收益 / 年化波动率 |
| 胜率 | 盈利交易占比 | 盈利次数 / 总交易次数 |
| 盈亏比 | 平均盈利/平均亏损 | 总盈利 / 总亏损 |

## 下一步

### 1. 接入真实日线数据

- 配置 mx-data HTTP API
- 或从 QMT/聚宽/Tushare 获取日线数据

### 2. 优化策略

- 参数优化（网格搜索、遗传算法）
- 多策略组合
- 动态仓位管理

### 3. 实盘对接

- 对接国金 QMT 实盘 API
- 实时信号生成
- 自动下单执行

## 文件结构

```
guojin_auto_trading/
├── backtest/
│   ├── backtest_daily_2023_2026.py    # 日线回测主程序
│   ├── engine.py                       # 通用回测引擎
│   ├── output/                         # 回测输出
│   │   ├── *_trades.csv
│   │   ├── *_records.csv
│   │   └── *_report.md
│   └── cache/
│       └── daily/                      # 日线数据缓存
├── strategies/
│   ├── rsi_four_stage_v4.py           # RSI 策略
│   ├── hybrid_monthly_daily_strategy.py  # 混合策略
│   └── ...
└── data/
    ├── mx_data_client.py              # mx-data 客户端
    └── ...
```

## 常见问题

### Q: 为什么回测结果只有 1 笔交易？

A: 当前使用的是月度数据（108 条），信号频率低。接入真正的日线数据（约 700 条）后交易次数会增加。

### Q: 如何修改交易标的？

A: 修改 `CONFIG['symbol']`，并确保有该标的的历史数据。

### Q: 如何添加新策略？

A: 继承 `DailyTradingStrategy` 类，重写 `generate_signal()` 方法。

---

**作者**: 量化策略组  
**版本**: V1.0.0  
**日期**: 2026-03-24
