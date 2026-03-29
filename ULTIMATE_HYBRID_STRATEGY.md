# 🚀 终极混合策略使用指南

**策略全称**: 宏观仓位控制 + RSI 四阶段 + 乖离率黄金分割混合策略  
**版本**: V1.0.0  
**创建日期**: 2026-03-26

---

## 📖 策略架构

```
┌─────────────────────────────────────────────────────────┐
│                    终极混合策略 V1.0                      │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  【月线级别】宏观仓位控制                                │
│  ├── 12 宏观因子打分 (0-12 分)                           │
│  ├── 决定总仓位上限 (0%/25%/50%/75%/100%)              │
│  └── 趋势过滤 (空头市场强制空仓)                         │
│                                                         │
│  【日线级别】仓位分配                                    │
│  ├── 底仓 60%：长期持有 (不错过趋势)                     │
│  └── 机动仓 40%：短线交易 (增强收益)                     │
│                                                         │
│  【技术指标】机动仓决策                                  │
│  ├── RSI 四阶段：吸筹/洗盘/拉升/出货                     │
│  ├── 乖离率：高抛低吸优化                               │
│  ├── 黄金分割：关键位置确认                             │
│  ├── MACD: 趋势确认                                     │
│  └── ADX: 趋势强度过滤                                  │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

## 🎯 核心逻辑

### 1. 宏观仓位控制（月线）

| 宏观得分 | 总仓位上限 | 市场状态 |
|---------|-----------|---------|
| 0-3 分 | 0% | 空仓观望 |
| 4-5 分 | 25% | 轻仓试探 |
| 6-7 分 | 50% | 半仓操作 |
| 8-9 分 | 75% | 重仓参与 |
| 10-12 分 | 100% | 满仓持有 |

**宏观因子包括**:
- TIPS 实际利率
- 美元指数
- CPI/PCE 通胀
- 央行购金
- COMEX 持仓
- VIX 恐慌指数
- 原油/黄金比
- 美欧利差
- 财政赤字
- PMI 数据
- ETF 持仓
- 美债利差

### 2. 仓位分配（日线）

```
总仓位 = 底仓 (60%) + 机动仓 (40%)

底仓:
├── 长期持有
├── 仅在宏观转空时清空
└── 确保不错过趋势行情

机动仓:
├── RSI 四阶段决定进出
├── 乖离率优化买卖点
├── 黄金分割确认位置
└── MACD + ADX 多重过滤
```

### 3. RSI 四阶段

| 阶段 | RSI(6) | RSI(12) | 操作 | 置信度 | 机动仓动作 |
|------|--------|---------|------|--------|-----------|
| 吸筹 | < 20 | - | 买入 | 90% | +30% |
| 洗盘 | 20-35 | RSI 短>长 | 买入 | 70% | +20% |
| 拉升 | 35-65 | RSI 短>长 | 持有 | 60% | 维持 |
| 出货 | > 80 | - | 卖出 | 90% | -30% |

### 4. 乖离率阈值

| 乖离率 (20 日) | 信号 | 机动仓调整 |
|---------------|------|-----------|
| > 25% | 严重高估 | -10% (清仓浮动仓) |
| > 15% | 高估 | -10% (部分止盈) |
| -5% ~ 10% | 合理 | 维持 |
| < -5% | 低估 | +10% (买入) |
| < -10% | 严重低估 | +10% (重仓买入) |

### 5. 黄金分割位

| 黄金分割比 | 位置 | 信号 | 机动仓调整 |
|-----------|------|------|-----------|
| ≥ 78.6% | 极度回调 | 黄金坑 | +10% |
| ≥ 61.8% | 极端回调 | 买入 | +10% |
| ≥ 50.0% | 深度回调 | 加仓 | +5% |
| ≥ 38.2% | 中度回调 | 买回 | +5% |
| ≥ 23.6% | 浅回调 | 观察 | 维持 |
| < 23.6% | 高位 | 谨慎 | -5% |

---

## 💻 使用方法

### 快速开始

```python
from strategies.hybrid_ultimate import UltimateHybridStrategy

# 1. 创建策略实例
config = {
    'base_ratio': 0.6,          # 底仓 60%
    'flexible_ratio': 0.4,      # 机动仓 40%
    'empty_threshold': 3,       # ≤3 分空仓
    'light_threshold': 5,       # 4-5 分轻仓
    'half_threshold': 7,        # 6-7 分半仓
    'heavy_threshold': 9,       # 8-9 分重仓
    'full_threshold': 10,       # ≥10 分满仓
}

strategy = UltimateHybridStrategy(config)

# 2. 每月初更新宏观仓位
macro_score = 8  # 从宏观策略获取
trend_state = "多头"
max_position = strategy.update_monthly_position(macro_score, trend_state)
print(f"最大仓位：{max_position*100:.0f}%")

# 3. 每日生成交易信号
signal = strategy.analyze_daily(
    current_price=9.641,          # 当前价格
    prices=price_list,            # 历史价格序列
    dates=date_list,              # 日期序列
    macro_score=8,                # 宏观得分
    macro_trend="多头",           # 宏观趋势
    high_60=10.5,                 # 60 日最高价
    low_60=9.0,                   # 60 日最低价
    ma20=9.8,                     # 20 日均线
    ma60=10.2,                    # 60 日均线
    macd_dif=0.05,                # MACD DIF
    macd_dea=0.03,                # MACD DEA
    macd_hist=0.02,               # MACD 柱状图
    adx=28.5                      # ADX 指标
)

# 4. 获取交易建议
print(signal.analysis)
print(f"最终操作：{signal.final_action}")
print(f"目标仓位：{signal.final_position*100:.0f}%")
```

### 输出示例

```
==================================================
【终极混合策略信号】
==================================================

📊 仓位配置:
  最大仓位：75%
  底仓：45% (长期持有)
  机动仓：30% (短线交易)
  目标总仓位：68%
  最终仓位：68%

📈 技术指标:
  RSI: 28.5(短) / 35.2(长) - 洗盘
  乖离率：-6.8% (20 日) / -4.2% (60 日)
  黄金分割：深度回调 (0.523)
  MACD: 金叉
  ADX: 28.5 (strong 趋势)

🎯 操作建议:
  底仓：持有底仓
  机动仓：机动仓重仓
  最终：重仓参与

📝 决策理由:
  • 宏观得分：8 → 最大仓位 75%
  • RSI 阶段：洗盘 (28.5/35.2)
  • 乖离率：-6.8% → 低估，买入
  • 黄金分割：深度回调 (0.523)
  • MACD: 金叉
  • ADX: 28.5 (strong 趋势)
==================================================
```

---

## 📊 回测

### 运行回测

```bash
# 进入策略目录
cd guojin_auto_trading

# 运行回测
python backtest/backtest_ultimate_hybrid.py
```

### 输出文件

| 文件 | 说明 |
|------|------|
| `backtest/output/ultimate_hybrid_trades.csv` | 交易记录 |
| `backtest/output/ultimate_hybrid_daily.csv` | 每日数据 |
| `backtest/output/ultimate_hybrid_performance.png` | 绩效图表 |
| `backtest/output/ultimate_hybrid_report.md` | Markdown 报告 |

---

## 🔧 参数配置

### 核心参数

```python
config = {
    # 仓位分配
    'base_ratio': 0.6,              # 底仓占比 (默认 60%)
    'flexible_ratio': 0.4,          # 机动仓占比 (默认 40%)
    
    # 宏观仓位阈值
    'empty_threshold': 3,           # 0-3 分空仓
    'light_threshold': 5,           # 4-5 分轻仓
    'half_threshold': 7,            # 6-7 分半仓
    'heavy_threshold': 9,           # 8-9 分重仓
    'full_threshold': 10,           # 10-12 分满仓
    
    # RSI 参数
    'rsi_short_period': 6,          # 短期 RSI 周期
    'rsi_long_period': 12,          # 长期 RSI 周期
    'oversold_threshold': 30,       # 超卖阈值
    'overbought_threshold': 70,     # 超买阈值
    'stage1_oversold': 20,          # 极度超卖
    'stage2_oversold': 35,          # 超卖
    'stage1_overbought': 80,        # 极度超买
    'stage2_overbought': 65,        # 超买
    
    # 乖离率参数
    'ma20_period': 20,              # 20 日均线
    'ma60_period': 60,              # 60 日均线
    'bias_sell_1': 15.0,            # 第一档止盈
    'bias_sell_2': 25.0,            # 第二档清仓
    'bias_buy_1': -5.0,             # 第一档买入
    'bias_buy_2': -10.0,            # 第二档重仓
    
    # 黄金分割
    'fib_high_period': 60,          # 高低点周期
    
    # MACD
    'macd_fast': 12,
    'macd_slow': 26,
    'macd_signal_period': 9,
    
    # ADX
    'adx_period': 14,
    'adx_strong_threshold': 25,
    'adx_weak_threshold': 20,
}
```

### 参数调优建议

#### 牛市环境
```python
config = {
    'base_ratio': 0.7,              # 提高底仓
    'flexible_ratio': 0.3,          # 降低机动仓
    'empty_threshold': 2,           # 更积极
    'full_threshold': 9,
}
```

#### 熊市环境
```python
config = {
    'base_ratio': 0.5,              # 降低底仓
    'flexible_ratio': 0.5,          # 提高机动仓灵活性
    'empty_threshold': 4,           # 更保守
    'full_threshold': 11,
}
```

#### 震荡市
```python
config = {
    'base_ratio': 0.5,
    'flexible_ratio': 0.5,
    'stage1_oversold': 25,          # 放宽 RSI 阈值
    'stage2_oversold': 40,
    'bias_buy_1': -3.0,             # 更敏感的乖离率
    'bias_buy_2': -7.0,
}
```

---

## 📈 实盘部署

### 每日工作流程

```
08:00  检查宏观数据更新
       └── 如果是月初，更新宏观得分和总仓位

15:30  收盘后获取当日数据
       └── 价格、成交量、技术指标

15:45  运行策略生成信号
       └── python backtest/backtest_ultimate_hybrid.py

16:00  执行交易
       └── 根据信号调整机动仓位

16:30  记录交易日志
       └── 更新持仓和绩效记录
```

### 自动化脚本

```bash
# 创建每日任务脚本 (daily_task.bat)
@echo off
cd /d C:\国金证券 QMT 交易端\guojin_auto_trading
python backtest\backtest_ultimate_hybrid.py
python core\execute_trades.py
echo 每日任务完成 >> logs\daily_log.txt
```

### 定时任务设置

**Windows 任务计划程序**:
1. 打开任务计划程序
2. 创建基本任务
3. 名称：终极混合策略每日任务
4. 触发器：每个交易日 15:45
5. 操作：启动程序 `daily_task.bat`

---

## ⚠️ 风险提示

### 策略风险

1. **宏观数据滞后**
   - 月度调仓可能错过快速市场变化
   - 建议：增加周度监控机制

2. **参数过拟合**
   - 回测参数可能在实盘失效
   - 建议：定期重新校准参数

3. **交易成本**
   - 频繁交易增加手续费
   - 建议：设置最小交易阈值

4. **流动性风险**
   - 极端市场可能无法成交
   - 建议：使用限价单 + 止损单

### 使用建议

1. **首次使用**: 先用模拟盘测试 1-3 个月
2. **资金管理**: 单笔交易不超过总资金 5%
3. **止损设置**: 建议设置 8% 硬止损
4. **定期回顾**: 每月回顾策略表现，必要时调整参数

---

## 📚 相关文件

| 文件 | 说明 |
|------|------|
| `strategies/hybrid_ultimate.py` | 策略主文件 |
| `backtest/backtest_ultimate_hybrid.py` | 回测脚本 |
| `docs/ULTIMATE_HYBRID_STRATEGY.md` | 使用文档 |
| `strategies/gold_trend_factor.py` | 宏观策略参考 |
| `strategies/rsi_four_stage.py` | RSI 策略参考 |

---

## 🆘 常见问题

### Q1: 宏观得分如何获取？

**A**: 可以使用以下数据源:
- `data/macro_api_unified.py` - 统一宏观数据 API
- `data/mx_data_client.py` - mx-data 客户端
- 手动输入（基于经济数据发布）

### Q2: 底仓和机动仓如何分离管理？

**A**: 策略内部自动计算:
```python
底仓 = 总仓位上限 × 60%
机动仓 = 总仓位上限 × 40%
```
实际交易时，底仓部分保持不变，仅调整机动仓。

### Q3: 如何与 QMT 集成？

**A**: 参考 `core/order_manager.py`:
```python
from xtquant import xttrader

# 获取策略信号
signal = strategy.analyze_daily(...)

# 执行交易
if signal.final_action == "重仓参与":
    xttrader.order_buy(stock_code, target_shares)
```

### Q4: 回测结果与实盘差异大？

**A**: 可能原因:
- 滑点未考虑（建议设置 0.1% 滑点）
- 交易成本未计算（佣金 + 印花税）
- 数据质量差异（使用真实数据回测）

---

**策略版本**: V1.0.0  
**最后更新**: 2026-03-26  
**维护者**: 量化策略组  
**联系方式**: 内部通讯
