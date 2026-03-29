# RSI 四阶段抄底逃顶策略（薛松版）

> 📌 **策略类型**: 日线级别趋势策略  
> 📌 **核心指标**: RSI(6) + RSI(12) + 量价关系 + 背离检测  
> 📌 **适用场景**: 抄底、逃顶、趋势跟踪

---

## 📋 策略原理

本策略基于薛松的 RSI 四阶段理论，将主力操盘的**吸筹、洗盘、拉升、出货**四步与 RSI 区间和形态对应，用于判断主力阶段与操作时机。

### 核心思想

```
主力操盘四步 → RSI 四阶段 → 交易信号

吸筹 (0-30)   → 超卖区底背离 → 买入
洗盘 (30-70)  → 震荡区       → 持股/观望
拉升 (50→70+) → 主升浪       → 持股待涨
出货 (70-100) → 超买区顶背离 → 卖出
```

---

## 🎯 RSI 四阶段详解

### 1️⃣ 吸筹阶段（超卖区：0-30）

| 特征 | 描述 |
|------|------|
| **RSI 区间** | 长期在 30 以下，常到 20 以下 |
| **形态信号** | 底背离（股价新低、RSI 不新低）、W 底/平底 |
| **主力行为** | 低位悄悄吸筹，制造恐慌让散户割肉 |
| **量价特征** | 温和放量 |
| **操作策略** | 分批低吸，不恐慌割肉 |

**买入信号**:
- ✅ RSI < 20（深超卖）
- ✅ 底背离 + RSI < 30（最强信号）
- ✅ 超卖区金叉（RSI 短线上穿长线）

---

### 2️⃣ 洗盘阶段（常态区：30-70）

| 特征 | 描述 |
|------|------|
| **RSI 区间** | 在 30-70 反复震荡 |
| **形态信号** | 短/长期 RSI 频繁金叉死叉，无明显趋势 |
| **主力行为** | 清洗浮筹，吓走不坚定筹码 |
| **量价特征** | 缩量 |
| **操作策略** | 持股不动，不被洗出 |

**信号**:
- 🟡 持股观望
- 🟢 洗盘结束信号：RSI 金叉上穿 50

---

### 3️⃣ 拉升阶段（常态→超买：50→70+）

| 特征 | 描述 |
|------|------|
| **RSI 区间** | 从 50 稳步上攻，突破 70 进入超买 |
| **形态信号** | 短 RSI 在长 RSI 之上，无顶背离 |
| **主力行为** | 主升浪，快速拉升 |
| **量价特征** | 放量 |
| **操作策略** | 持股待涨，回踩 50 附近可加仓 |

**信号**:
- 🟡 持股待涨（RSI > 50，短线>长线）
- 🔴 警惕信号：拉升区死叉
- ⚪ 准备逃顶：RSI > 80

---

### 4️⃣ 出货阶段（超买区：70-100）

| 特征 | 描述 |
|------|------|
| **RSI 区间** | 在 80+ 高位钝化 |
| **形态信号** | 顶背离（股价新高、RSI 不新高）、拐头向下 |
| **主力行为** | 高位派发筹码 |
| **量价特征** | 放量震荡 |
| **操作策略** | 分批减仓/清仓，不追高 |

**卖出信号**:
- 🔴 RSI > 85（深超买）
- 🔴 顶背离 + RSI > 70（最强信号）
- 🔴 超买区死叉（RSI 短线下穿长线）

---

## 📊 核心区间速记

```
┌─────────────────────────────────────────────────────────────┐
│  RSI 区间    │  阶段    │  信号    │  操作                  │
├─────────────────────────────────────────────────────────────┤
│  0-20       │  深超卖  │  🟢 买入  │  分批低吸              │
│  20-30      │  超卖    │  🟢 买入  │  关注底背离            │
│  30-50      │  洗盘下  │  🟡 观望  │  持股不动              │
│  50-70      │  洗盘上  │  🟡 持股  │  关注金叉              │
│  70-80      │  超买    │  🟡 警惕  │  准备减仓              │
│  80-100     │  深超买  │  🔴 卖出  │  分批清仓              │
└─────────────────────────────────────────────────────────────┘

关键点位:
- 30: 超卖/超买分界线
- 50: 多空分界线，上多下空
- 70: 超买阈值
- 20/80: 深超卖/深超买
```

---

## 🔧 策略参数

### 默认配置

```yaml
# config/strategies.yaml

rsi_four_stage:
  # RSI 周期
  rsi_short_period: 6          # 短期 RSI（薛松推荐 6 日）
  rsi_long_period: 12          # 长期 RSI（薛松推荐 12 日）
  
  # 阈值
  oversold_threshold: 30       # 超卖阈值
  overbought_threshold: 70     # 超买阈值
  
  # 量价分析
  volume_ma_period: 20         # 成交量均线周期
  volume_spike_ratio: 1.5      # 放量倍数（>1.5 为放量）
  
  # 背离检测
  divergence_lookback: 30      # 背离检测回溯天数
  divergence_min_bars: 5       # 背离最小间隔
```

### 参数调优建议

| 参数 | 保守 | 默认 | 激进 |
|------|------|------|------|
| rsi_short_period | 9 | 6 | 5 |
| rsi_long_period | 14 | 12 | 10 |
| oversold_threshold | 25 | 30 | 35 |
| overbought_threshold | 75 | 70 | 65 |
| divergence_lookback | 45 | 30 | 20 |

---

## 💻 使用示例

### 基础用法

```python
from strategies.rsi_four_stage import RSIFourStageStrategy

# 创建策略
config = {
    'rsi_short_period': 6,
    'rsi_long_period': 12,
    'oversold_threshold': 30,
    'overbought_threshold': 70,
}
strategy = RSIFourStageStrategy(config)

# 获取 K 线数据（日线）
klines = await api_client.get_kline('600519.SH', '1d', 60)

# 分析信号
signal = strategy.analyze('600519.SH', klines)

if signal:
    print(f"阶段：{signal.stage.value}")
    print(f"信号：{signal.signal_type.value}")
    print(f"RSI: {signal.rsi_short}/{signal.rsi_long}")
    print(f"原因：{signal.reason}")
    print(f"置信度：{signal.confidence*100:.1f}%")
```

### 批量选股

```python
async def scan_stocks(strategy, stock_list, api_client):
    """批量扫描股票 RSI 信号"""
    buy_signals = []
    sell_signals = []
    
    for symbol in stock_list:
        klines = await api_client.get_kline(symbol, '1d', 60)
        if not klines:
            continue
        
        signal = strategy.analyze(symbol, klines)
        if signal:
            if signal.signal_type == SignalType.BUY:
                buy_signals.append(signal)
            elif signal.signal_type == SignalType.SELL:
                sell_signals.append(signal)
    
    # 按置信度排序
    buy_signals.sort(key=lambda x: x.confidence, reverse=True)
    sell_signals.sort(key=lambda x: x.confidence, reverse=True)
    
    return buy_signals, sell_signals

# 使用
buy_list, sell_list = await scan_stocks(strategy, stock_pool, api_client)

print("🟢 买入信号:")
for s in buy_list[:5]:
    print(f"  {s.symbol}: {s.reason} (置信度{s.confidence*100:.0f}%)")

print("🔴 卖出信号:")
for s in sell_list[:5]:
    print(f"  {s.symbol}: {s.reason} (置信度{s.confidence*100:.0f}%)")
```

### 集成到主系统

```python
# main.py

from strategies.rsi_four_stage import RSIFourStageStrategy, SignalType

class TradingSystem:
    def __init__(self, config):
        # ... 其他初始化
        
        # 初始化 RSI 策略
        rsi_config = config.get('strategies', {}).get('rsi_four_stage', {})
        self.rsi_strategy = RSIFourStageStrategy(rsi_config, self.api_client)
    
    async def _main_loop(self):
        # ... 其他逻辑
        
        # RSI 策略信号
        for symbol in self.watch_list:
            klines = await self.api_client.get_kline(symbol, '1d', 60)
            signal = self.rsi_strategy.analyze(symbol, klines)
            
            if signal:
                if signal.signal_type == SignalType.BUY and signal.confidence > 0.7:
                    logger.info(f"RSI 买入信号：{symbol} - {signal.reason}")
                    # 执行买入
                    
                elif signal.signal_type == SignalType.SELL and signal.confidence > 0.7:
                    logger.warning(f"RSI 卖出信号：{symbol} - {signal.reason}")
                    # 执行卖出
```

---

## 📈 信号强度评估

### 买入信号强度

| 信号组合 | 置信度 | 说明 |
|---------|--------|------|
| 底背离 + RSI<20 | 90%+ | 最强买入信号 |
| 底背离 + RSI<30 | 80%+ | 强买入信号 |
| RSI<20（无背离） | 70% | 深超卖买入 |
| 超卖区金叉 | 65% | 技术性买入 |
| 吸筹阶段观望 | 50% | 分批低吸 |

### 卖出信号强度

| 信号组合 | 置信度 | 说明 |
|---------|--------|------|
| 顶背离 + RSI>80 | 90%+ | 最强卖出信号 |
| 顶背离 + RSI>70 | 80%+ | 强卖出信号 |
| RSI>85（无背离） | 75% | 深超卖卖出 |
| 超买区死叉 | 70% | 技术性卖出 |
| 出货阶段减仓 | 60% | 分批清仓 |

---

## 🧪 回测要点

### 回测配置

```python
from backtest.engine import BacktestEngine, BacktestConfig
from strategies.rsi_four_stage import RSIFourStageStrategy

# 回测配置
config = BacktestConfig(
    start_date='2023-01-01',
    end_date='2024-12-31',
    initial_cash=1000000,
    commission_rate=0.0003,
    stamp_tax_rate=0.001,
    slippage=0.002
)

engine = BacktestEngine(config)

# RSI 策略
rsi_strategy = RSIFourStageStrategy({
    'rsi_short_period': 6,
    'rsi_long_period': 12,
})

# 运行回测
result = engine.run(rsi_strategy)

print(f"总收益：{result.total_return*100:.2f}%")
print(f"年化收益：{result.annual_return*100:.2f}%")
print(f"最大回撤：{result.max_drawdown*100:.2f}%")
print(f"夏普比率：{result.sharpe_ratio:.2f}")
print(f"胜率：{result.win_rate*100:.1f}%")
```

### 回测优化方向

1. **参数优化**: 测试不同 RSI 周期组合
2. **置信度阈值**: 调整信号执行阈值（默认 0.7）
3. **仓位管理**: 根据信号强度调整仓位
4. **止损止盈**: 结合 RSI 阶段设置动态止损

---

## ⚠️ 注意事项

### 1. 数据要求

- **最小 K 线数**: 至少 30 条日线数据（用于背离检测）
- **数据质量**: 确保价格、成交量数据准确
- **复权处理**: 建议使用前复权数据

### 2. 适用场景

| 市场状态 | 适用性 | 说明 |
|---------|--------|------|
| 震荡市 | ⭐⭐⭐⭐⭐ | RSI 震荡策略效果最佳 |
| 慢牛市 | ⭐⭐⭐⭐ | 拉升阶段持股收益高 |
| 快牛市 | ⭐⭐⭐ | 容易过早卖出 |
| 熊市 | ⭐⭐⭐⭐ | 吸筹阶段抄底机会多 |
| 暴跌市 | ⭐⭐ | RSI 可能长期超卖 |

### 3. 局限性

- **滞后性**: RSI 是滞后指标，信号可能延迟
- **钝化**: 强势股 RSI 可能长期超买不回调
- **假信号**: 震荡区金叉死叉频繁，需结合其他指标
- **背离失效**: 强势行情中背离可能多次失效

### 4. 建议配合指标

```
RSI 四阶段 + 

✅ 均线系统（MA20/MA60/MA125）- 确认趋势
✅ MACD - 辅助背离确认
✅ 成交量 - 量价配合验证
✅ KDJ - 短期超买超卖补充
✅ 布林带 - 波动率参考
```

---

## 📚 实战技巧（薛松常用）

### 技巧 1: 背离优先

> "背离比单纯超买超卖更准"

- 底背离 > RSI<30
- 顶背离 > RSI>70
- 多次背离（二次、三次）信号更强

### 技巧 2: 50 中轴法则

> "50 是多空分界线，上多下空"

- RSI 上穿 50：多头信号，可加仓
- RSI 下穿 50：空头信号，减仓
- RSI 在 50 上方运行：持股为主

### 技巧 3: 量价配合

> "吸筹温和放量、洗盘缩量、拉升放量、出货放量震荡"

- 吸筹区 + 温和放量 = 确认吸筹
- 洗盘区 + 明显缩量 = 洗盘接近尾声
- 拉升区 + 持续放量 = 主升浪延续
- 出货区 + 放量滞涨 = 警惕见顶

### 技巧 4: 分仓操作

> "不all in，分批进出"

- 吸筹区：分 3-5 批建仓
- 出货区：分 3-5 批减仓
- 避免一次性满仓/空仓

---

## 🔗 相关文件

| 文件 | 说明 |
|------|------|
| `strategies/rsi_four_stage.py` | 策略实现 |
| `examples/rsi_four_stage_example.py` | 使用示例 |
| `config/strategies.yaml` | 策略配置 |

---

## 📞 参考资源

- 薛松 RSI 四阶段理论原文
- 《技术分析基础》- 约翰·墨菲
- [聚宽 RSI 策略研究](https://www.joinquant.com/)

---

**最后更新**: 2026-03-21  
**版本**: 1.0.0
