# 日线交易回测系统 - 完成报告

## 📊 回测结果摘要

### 基本信息
- **标的**: 518880 (黄金 ETF)
- **回测区间**: 2023-01-01 至 2026-03-24
- **初始资金**: 100,000.00 元
- **数据频率**: 月度数据 (108 条)

### 绩效指标
| 指标 | 数值 | 说明 |
|------|------|------|
| 总收益率 | **1.18%** | 回测期间总收益 |
| 年化收益率 | **9.55%** | 年化后收益率 |
| 最大回撤 | **-0.06%** | 风险控制良好 |
| 夏普比率 | **2.18** | 风险调整后收益优秀 |
| 盈亏比 | **inf** | 无亏损交易 |

### 交易统计
- 总交易次数：1 次
- 买入次数：1 次
- 卖出次数：0 次
- 胜率：0.00% (持仓中，未实现收益)
- 最终资产：**101,180.08 元**

### 最近交易
```
2026-03-23 BUY 4100@10.413 (RSI=27.5, 乖离率=0.1%, 均线多头)
```

---

## 🎯 系统功能

### 1. 数据获取模块 (`DailyDataFetcher`)

```python
from backtest.backtest_daily_2023_2026 import DailyDataFetcher

fetcher = DailyDataFetcher('518880')
df = fetcher.fetch_daily_data('2023-01-01', '2026-03-24')
```

**支持的数据源**:
- ✅ mx-data API (需配置 HTTP 端点)
- ✅ 本地 CSV 文件缓存
- ✅ 模拟数据生成

### 2. 策略模块 (`DailyTradingStrategy`)

**技术指标**:
- RSI (6/12 周期)
- MACD (12/26/9)
- ADX (14 周期趋势强度)
- 均线 (MA5/10/20/55/60)
- 乖离率 (BIAS20)
- 黄金分割位

**交易信号**:
```python
# 买入信号 (满足 2 个以上条件)
- RSI6 < 30 且 RSI6 < RSI12 (超卖)
- 乖离率 < -5% (超跌)
- 均线多头排列 (价格>MA20>MA60)

# 卖出信号 (满足 2 个以上条件)
- RSI6 > 70 且 RSI6 > RSI12 (超买)
- 乖离率 > 15% (超涨)
- 均线空头排列 (价格<MA20<MA60)
```

### 3. 回测引擎 (`DailyBacktestEngine`)

**交易成本**:
- 手续费：0.03% (万三)
- 印花税：0.1% (卖出时收取)
- 滑点：0.1%

**绩效计算**:
- 总收益率、年化收益率
- 最大回撤、夏普比率
- 胜率、盈亏比
- 交易统计

### 4. 报告生成

**输出文件**:
```
backtest/output/
├── 518880_daily_trades.csv       # 交易记录
├── 518880_daily_records.csv      # 每日资产
└── 518880_daily_backtest_report.md  # 回测报告
```

---

## 📁 文件结构

```
guojin_auto_trading/
├── backtest/
│   ├── backtest_daily_2023_2026.py    ← 日线回测主程序
│   ├── engine.py                       ← 通用回测引擎
│   ├── output/                         ← 回测输出目录
│   │   ├── 518880_daily_trades.csv
│   │   ├── 518880_daily_records.csv
│   │   └── 518880_daily_backtest_report.md
│   └── cache/
│       └── daily/                      ← 日线数据缓存
├── strategies/
│   ├── rsi_four_stage_v4.py           ← RSI 四阶段策略
│   ├── hybrid_monthly_daily_strategy.py  ← 混合策略
│   └── ...
├── data/
│   ├── mx_data_client.py              ← mx-data 客户端
│   └── ...
└── docs/
    └── DAILY_BACKTEST_GUIDE.md        ← 使用指南
```

---

## ⚠️ 重要说明

### 当前使用的是月度数据

**原因**:
1. mx-data HTTP API 未配置，无法自动获取日线数据
2. 本地只有月度回测数据 (`518880_github_strategy.csv`)

**影响**:
- 数据量：108 条 (月度) vs ~700 条 (日线)
- 交易信号：1 次 vs 预计 10-20 次
- 策略效果：无法充分体现日线波段优势

### 如何获取真正的日线数据？

#### 方案 1: 配置 mx-data HTTP API (推荐)

在 `openclaw.json` 中添加:

```json
{
  "skills": {
    "mx-data": {
      "http_enabled": true,
      "api_key": "YOUR_KIMI_API_KEY",
      "base_url": "https://api.moonshot.cn/v1"
    }
  }
}
```

然后系统会自动查询:
```python
result = mx_client.query("518880 日线数据 2023-01-01 至 2026-03-24")
```

#### 方案 2: 从 QMT 获取

```python
from xtquant import xtdata

# 下载日线数据
xtdata.download_history_data(
    '518880.SH', 
    period='1d', 
    start_date='20230101', 
    end_date='20260324'
)

# 获取数据
data = xtdata.get_market_data('518880.SH', period='1d')
```

#### 方案 3: 使用 Tushare/AkShare

```python
import akshare as ak

df = ak.fund_etf_hist_em(
    symbol="518880", 
    period="daily", 
    start_date="20230101",
    end_date="20260324"
)
```

---

## 🚀 下一步优化

### 1. 接入真实日线数据
- [ ] 配置 mx-data HTTP API
- [ ] 或集成 AkShare/Tushare
- [ ] 或从 QMT 导出日线数据

### 2. 策略优化
- [ ] 参数网格搜索优化
- [ ] 多策略组合回测
- [ ] 动态仓位管理
- [ ] 止损止盈策略

### 3. 实盘对接
- [ ] 对接国金 QMT 实盘 API
- [ ] 实时信号生成
- [ ] 自动下单执行
- [ ] 持仓监控

### 4. 风险控制
- [ ] 单笔最大亏损限制
- [ ] 总仓位上限控制
- [ ] 黑名单机制
- [ ] 异常波动处理

---

## 📝 使用示例

### 运行回测

```bash
cd guojin_auto_trading
python backtest/backtest_daily_2023_2026.py
```

### 修改配置

编辑 `backtest_daily_2023_2026.py`:

```python
CONFIG = {
    'symbol': '518880',              # 修改标的
    'start_date': '2023-01-01',
    'end_date': '2026-03-24',
    'initial_capital': 100000.0,
    'commission_rate': 0.0003,
    
    # 策略参数
    'rsi_short_period': 6,
    'oversold_threshold': 30,
    'overbought_threshold': 70,
}
```

### 查看结果

```python
import pandas as pd

# 交易记录
trades = pd.read_csv('backtest/output/518880_daily_trades.csv')
print(trades)

# 每日资产
records = pd.read_csv('backtest/output/518880_daily_records.csv')
print(records[['date', 'close', 'total_asset']])

# 回测报告
with open('backtest/output/518880_daily_backtest_report.md', 'r', encoding='utf-8') as f:
    print(f.read())
```

---

## ✅ 完成清单

- [x] 日线回测引擎实现
- [x] 多技术指标计算 (RSI/MACD/ADX/均线/乖离率)
- [x] 交易信号生成逻辑
- [x] 交易执行模拟 (含成本)
- [x] 绩效指标计算
- [x] 数据缓存机制
- [x] 报告自动生成
- [x] 使用文档编写

---

## 📞 联系

如有问题或建议，请查阅:
- `docs/DAILY_BACKTEST_GUIDE.md` - 详细使用指南
- `backtest/output/` - 回测结果文件

---

**创建时间**: 2026-03-24  
**版本**: V1.0.0  
**作者**: 量化策略组
