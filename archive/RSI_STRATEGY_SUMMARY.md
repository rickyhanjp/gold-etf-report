# RSI 四阶段抄底逃顶策略 - 完整实现总结

> 📅 **创建时间**: 2026-03-21  
> 📚 **理论基础**: 薛松 RSI 四阶段理论  
> 📊 **策略级别**: 日线  
> 🎯 **核心功能**: 抄底、逃顶、趋势跟踪

---

## 📁 已创建文件

| 文件 | 路径 | 说明 |
|------|------|------|
| **策略核心** | `strategies/rsi_four_stage.py` | RSI 四阶段策略完整实现 |
| **使用示例** | `examples/rsi_four_stage_example.py` | 5 个演示场景 |
| **策略文档** | `docs/RSI_FOUR_STAGE.md` | 详细使用指南 |
| **回测脚本** | `backtest/rsi_backtest.py` | 回测 + 参数对比 |
| **配置文件** | `config/strategies.yaml` | 已添加 RSI 策略配置 |
| **总结文档** | `RSI_STRATEGY_SUMMARY.md` | 本文档 |

---

## 🎯 策略核心功能

### 1. RSI 四阶段识别

```
┌─────────────────────────────────────────────────────────────┐
│  阶段      │  RSI 区间  │  特征              │  操作        │
├─────────────────────────────────────────────────────────────┤
│  📥 吸筹   │  0-30     │  超卖、底背离      │  分批买入    │
│  🔄 洗盘   │  30-70    │  震荡、金叉死叉    │  持股观望    │
│  📈 拉升   │  50→70+   │  主升浪、强势      │  持股待涨    │
│  📤 出货   │  70-100   │  超买、顶背离      │  分批卖出    │
└─────────────────────────────────────────────────────────────┘
```

### 2. 背离检测

- **底背离**: 股价创新低，RSI 不创新低 → 强买入信号
- **顶背离**: 股价创新高，RSI 不创新高 → 强卖出信号
- 背离强度量化 (0-1)

### 3. 量价分析

| 阶段 | 量价特征 |
|------|---------|
| 吸筹 | 温和放量 |
| 洗盘 | 缩量 |
| 拉升 | 放量 |
| 出货 | 放量震荡 |

### 4. 信号置信度

| 信号类型 | 置信度范围 | 说明 |
|---------|-----------|------|
| 底背离 + RSI<20 | 80-100% | 最强买入 |
| 顶背离 + RSI>80 | 80-100% | 最强卖出 |
| 深超卖/深超买 | 70-75% | 强信号 |
| 金叉/死叉 | 65-70% | 中等信号 |
| 阶段判断 | 50-60% | 基础信号 |

---

## 🚀 快速开始

### 1. 基础使用

```python
from strategies.rsi_four_stage import RSIFourStageStrategy

# 创建策略
strategy = RSIFourStageStrategy({
    'rsi_short_period': 6,
    'rsi_long_period': 12,
})

# 分析股票
klines = await api_client.get_kline('600519.SH', '1d', 60)
signal = strategy.analyze('600519.SH', klines)

print(f"阶段：{signal.stage.value}")
print(f"信号：{signal.signal_type.value}")
print(f"原因：{signal.reason}")
```

### 2. 运行示例

```bash
cd guojin_auto_trading
python examples/rsi_four_stage_example.py
```

### 3. 运行回测

```bash
python backtest/rsi_backtest.py
```

---

## 📊 策略配置

### config/strategies.yaml

```yaml
strategies:
  rsi_four_stage:
    enabled: true
    
    # RSI 周期
    rsi_short_period: 6
    rsi_long_period: 12
    
    # 阈值
    oversold_threshold: 30
    overbought_threshold: 70
    
    # 量价
    volume_ma_period: 20
    volume_spike_ratio: 1.5
    
    # 背离
    divergence_lookback: 30
    
    # 交易
    min_confidence: 0.7
    mode: "signal"  # signal | auto
```

---

## 🎯 交易信号逻辑

### 买入信号（按强度排序）

| 强度 | 条件 | 置信度 |
|------|------|--------|
| ⭐⭐⭐⭐⭐ | 底背离 + RSI<30 | 80-100% |
| ⭐⭐⭐⭐ | RSI<20（深超卖） | 70% |
| ⭐⭐⭐ | 超卖区金叉 | 65% |
| ⭐⭐ | 洗盘区底背离 | 70% |
| ⭐ | 洗盘结束（上穿 50） | 60% |

### 卖出信号（按强度排序）

| 强度 | 条件 | 置信度 |
|------|------|--------|
| ⭐⭐⭐⭐⭐ | 顶背离 + RSI>70 | 80-100% |
| ⭐⭐⭐⭐ | RSI>85（深超买） | 75% |
| ⭐⭐⭐ | 超买区死叉 | 70% |
| ⭐⭐ | 拉升区死叉 | 60% |
| ⭐ | 出货阶段减仓 | 60% |

---

## 📈 集成到主系统

### main.py 集成示例

```python
from strategies.rsi_four_stage import RSIFourStageStrategy, SignalType

class TradingSystem:
    async def initialize(self):
        # ... 其他初始化
        
        # 初始化 RSI 策略
        rsi_config = self.config.get('strategies', {}).get('rsi_four_stage', {})
        self.rsi_strategy = RSIFourStageStrategy(rsi_config, self.api_client)
    
    async def _main_loop(self):
        # ... 其他逻辑
        
        # RSI 策略扫描
        for symbol in self.watch_list:
            klines = await self.api_client.get_kline(symbol, '1d', 60)
            signal = self.rsi_strategy.analyze(symbol, klines)
            
            if signal and signal.confidence >= 0.7:
                if signal.signal_type == SignalType.BUY:
                    logger.info(f"🟢 RSI 买入：{symbol} - {signal.reason}")
                    # 执行买入逻辑
                    
                elif signal.signal_type == SignalType.SELL:
                    logger.warning(f"🔴 RSI 卖出：{symbol} - {signal.reason}")
                    # 执行卖出逻辑
```

---

## 🧪 测试场景

示例脚本包含 5 个演示场景：

1. **基础用法** - 完整信号分析
2. **阶段识别** - 8 种 RSI 场景测试
3. **背离检测** - 底背离/顶背离识别
4. **量价分析** - 4 种量价状态
5. **完整周期** - 四阶段信号演示

---

## 📋 回测功能

### 回测脚本功能

- ✅ 完整回测引擎集成
- ✅ 止损止盈逻辑（5%/10%）
- ✅ 交易日志记录
- ✅ 绩效指标计算
- ✅ 参数对比测试

### 回测指标

| 指标 | 说明 |
|------|------|
| 总收益率 | 回测期总收益 |
| 年化收益 | 年化收益率 |
| 最大回撤 | 最大资产回撤 |
| 夏普比率 | 风险调整后收益 |
| 胜率 | 盈利交易占比 |
| 盈亏比 | 平均盈利/平均亏损 |

---

## ⚠️ 注意事项

### 数据要求

- 最小 K 线数：≥30 条（背离检测需要）
- 建议使用前复权数据
- 确保成交量数据准确

### 适用市场

| 市场状态 | 适用性 | 建议 |
|---------|--------|------|
| 震荡市 | ⭐⭐⭐⭐⭐ | 最佳场景 |
| 慢牛市 | ⭐⭐⭐⭐ | 持股待涨 |
| 快牛市 | ⭐⭐⭐ | 可能过早卖出 |
| 熊市 | ⭐⭐⭐⭐ | 抄底机会多 |
| 暴跌市 | ⭐⭐ | RSI 可能长期超卖 |

### 建议配合指标

```
RSI 四阶段 + 
✅ MA20/MA60/MA125 - 趋势确认
✅ MACD - 背离辅助
✅ 成交量 - 量价验证
✅ KDJ - 短期补充
```

---

## 📚 薛松实战要点

1. **背离优先**: 背离比单纯超买超卖更准
2. **50 中轴**: 上穿 50 加仓，下穿 50 减仓
3. **量价配合**: 吸筹放量、洗盘缩量、拉升放量、出货震荡
4. **分仓操作**: 分批进出，不 all in

---

## 🔗 相关文档

- [策略详细文档](./docs/RSI_FOUR_STAGE.md)
- [API 使用指南](./API_GUIDE.md)
- [快速开始](./QUICKSTART.md)
- [技术选股模块](./docs/TECHNICAL_SELECTOR.md)

---

## 📞 下一步

1. ✅ 策略已实现
2. ✅ 文档已完成
3. ✅ 示例已创建
4. ⏭️ 使用实盘数据回测
5. ⏭️ 调整参数优化
6. ⏭️ 集成到主交易系统
7. ⏭️ 模拟盘测试
8. ⏭️ 小资金实盘

---

**策略状态**: ✅ 完成  
**版本**: 1.0.0  
**最后更新**: 2026-03-21
