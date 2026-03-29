# 黄金趋势 + 因子打分策略使用说明

## 策略核心逻辑

### 1. 趋势过滤（第一道门槛）

**条件（满足任一即可）：**
- ✅ 黄金价格站上 50 日均线 + 200 日均线多头排列（MA50 > MA200）
- ✅ 月度收盘价 > 12 个月移动平均线

**结果：**
- 满足 = 允许做多
- 不满足 = 0 仓位（强制空仓）

### 2. 12 个宏观因子打分（每月一次）

| # | 因子 | 方向 | 得分条件 |
|---|------|------|----------|
| 1 | 美国 10 年期 TIPS 实际利率 | 正向 | 下行 = +1 分 |
| 2 | 美元指数 | 正向 | 月度下跌 = +1 分 |
| 3 | 美国核心 CPI/PCE | 正向 | 温和上行 (2-4%) = +1 分 |
| 4 | 全球央行净购金 | 正向 | 净购金 = +1 分 |
| 5 | COMEX 黄金投机净多头 | 正向 | 非超买 (<85%) = +1 分 |
| 6 | VIX 恐慌指数 | 正向 | 上升 = +1 分 |
| 7 | 原油/黄金比价 | 正向 | 上行 = +1 分 |
| 8 | 美欧实际利差 | 正向 | 收窄 = +1 分 |
| 9 | 美国财政赤字率 | 正向 | 扩大 = +1 分 |
| 10 | 全球制造业 PMI | 正向 | 弱于预期 = +1 分 |
| 11 | 黄金 ETF 持仓 | 正向 | 环比增加 = +1 分 |
| 12 | 美债期限利差 (10Y-2Y) | 正向 | 倒挂/收窄 = +1 分 |

**总分范围：0～12 分**

### 3. 动态仓位管理

| 得分 | 仓位 | 操作 |
|------|------|------|
| 0～4 分 | 0% | 空仓 |
| 5～7 分 | 50% | 半仓 |
| 8～12 分 | 100% | 满仓 |

**重要：** 必须同时满足趋势条件才开仓，否则一律空仓！

## 使用示例

### 基础用法

```python
from strategies.gold_trend_factor import GoldTrendFactorStrategy

# 初始化策略
strategy = GoldTrendFactorStrategy()

# 准备市场数据
market_data = {
    # TIPS 实际利率（%）
    'tips_current': 1.85,
    'tips_previous': 2.10,
    
    # 美元指数
    'dxy_current': 103.5,
    'dxy_previous': 104.2,
    
    # 核心 CPI（%）
    'cpi_current': 3.2,
    'cpi_previous': 3.1,
    
    # 央行净购金（吨）
    'central_bank_buying': 250,
    
    # COMEX 净多头比例（0-1）
    'comex_net_long': 0.72,
    
    # VIX
    'vix_current': 18.5,
    'vix_previous': 16.2,
    
    # 原油/黄金比价
    'oil_gold_ratio_current': 0.035,
    'oil_gold_ratio_previous': 0.033,
    
    # 美欧实际利差（%）
    'us_eu_spread_current': 1.2,
    'us_eu_spread_previous': 1.5,
    
    # 财政赤字率（%）
    'fiscal_deficit_current': 6.8,
    'fiscal_deficit_previous': 6.2,
    
    # 制造业 PMI
    'pmi_actual': 48.5,
    'pmi_expected': 50.0,
    
    # ETF 持仓（吨）
    'etf_holdings_current': 2850,
    'etf_holdings_previous': 2820,
    
    # 美债期限利差（10Y-2Y，%）
    'treasury_spread_current': -0.45,
    'treasury_spread_previous': -0.30,
}

# 执行分析
signal = strategy.analyze(
    current_price=2035,      # 当前金价
    ma50=2015,               # 50 日均线
    ma200=1980,              # 200 日均线
    monthly_close=2040,      # 月度收盘价
    ma12=1990,               # 12 月均线
    market_data=market_data
)

# 输出结果
print(f"趋势状态：{signal.trend_state.value}")
print(f"因子得分：{signal.total_score}/12")
print(f"仓位等级：{signal.position_level.value}")
print(f"仓位比例：{signal.position_ratio * 100}%")
print(f"操作建议：{signal.action}")
print(f"\n详细分析:\n{signal.analysis}")
```

### 输出示例

```
趋势状态：多头趋势
因子得分：8/12
仓位等级：满仓
仓位比例：100%
操作建议：满仓做多（100% 仓位）

详细分析:
【趋势过滤】
  ✅ 趋势条件满足，允许做多

【因子打分】总分：8/12
  ✅ 正向因子 (8 个):
     - 美国 10 年期 TIPS 实际利率
     - 美元指数
     - 美国核心 CPI/PCE
     - 全球央行净购金
     - COMEX 黄金投机净多头
     - VIX 恐慌指数
     - 美欧实际利差
     - 黄金 ETF 持仓
  ❌ 负向因子 (4 个):
     - 原油/黄金比价
     - 美国财政赤字率
     - 全球制造业 PMI
     - 美债期限利差

【仓位建议】
  仓位等级：满仓
  仓位比例：100%
```

## 自定义配置

```python
# 自定义配置
config = {
    # 均线周期
    'ma50_period': 50,
    'ma200_period': 200,
    'ma12_period': 12,
    
    # 趋势过滤阈值（价格需在均线上方百分之几）
    'trend_threshold': 0.02,  # 2%
    
    # 仓位管理阈值（可调整）
    'empty_threshold': 4,     # 0-4 分空仓
    'half_threshold': 7,      # 5-7 分半仓
    'full_threshold': 8,      # 8-12 分满仓
}

strategy = GoldTrendFactorStrategy(config=config)
```

## 回测示例

```python
from strategies.gold_trend_factor import GoldTrendFactorStrategy, GoldTrendFactorBacktest

# 准备历史数据
historical_data = [
    {
        'date': '2024-01-31',
        'price': 2035,
        'ma50': 2015,
        'ma200': 1980,
        'monthly_close': 2040,
        'ma12': 1990,
        'market_data': { ... }  # 因子数据
    },
    # ... 更多历史数据
]

# 运行回测
strategy = GoldTrendFactorStrategy()
backtest = GoldTrendFactorBacktest(strategy)
results = backtest.run_backtest(historical_data)

# 输出回测结果
print(f"总收益率：{results['total_return']}%")
print(f"最大回撤：{results['max_drawdown']}%")
print(f"胜率：{results['win_rate']}%")
print(f"总交易次数：{results['total_trades']}")
```

## 数据源说明

### 技术指标（日线/月线）
- 黄金价格：现货黄金（XAU/USD）
- 均线：自行计算或使用交易软件

### 宏观因子数据源
| 因子 | 推荐数据源 | 更新频率 |
|------|-----------|---------|
| TIPS 实际利率 | 美联储/财政部 | 每日 |
| 美元指数 |  ICE/外汇市场 | 每日 |
| 核心 CPI/PCE | 美国劳工部/商务部 | 月度 |
| 央行购金 | 世界黄金协会 (WGC) | 月度 |
| COMEX 持仓 | CFTC COT 报告 | 周度 |
| VIX | CBOE | 每日 |
| 原油/黄金比价 | 自行计算 | 每日 |
| 美欧利差 | 各国央行/彭博 | 每日 |
| 财政赤字率 | 美国财政部 | 月度 |
| 制造业 PMI | 标普/ISM | 月度 |
| ETF 持仓 | 世界黄金协会 | 每日 |
| 美债期限利差 | 美国财政部 | 每日 |

## 注意事项

1. **趋势优先**：趋势条件不满足时，因子得分再高也空仓
2. **月度调仓**：因子打分建议每月执行一次（月末或月初）
3. **数据质量**：确保宏观数据来自权威来源，避免滞后或错误
4. **风险管理**：建议配合止损策略使用
5. **参数优化**：可根据历史回测调整仓位阈值

## 策略优势

✅ **客观机械**：避免主观判断，严格执行规则
✅ **双重过滤**：趋势 + 因子，降低假信号
✅ **灵活仓位**：根据市场状况动态调整
✅ **全面覆盖**：12 个因子涵盖主要驱动因素

## 策略局限

⚠️ **数据依赖**：需要准确的宏观数据
⚠️ **滞后性**：部分宏观数据发布滞后
⚠️ **极端行情**：黑天鹅事件可能失效
⚠️ **参数敏感**：阈值设置影响表现

## 版本历史

- **V1.0.0** (2026-03-22): 初始版本，实现基础策略逻辑
