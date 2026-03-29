# 技术面选股模块详解

## 📋 9 大技术面条件

本选股模块实现了用户配置的 9 大技术面条件，所有条件必须同时满足才会被选中。

```
┌─────────────────────────────────────────────────────────────────┐
│  条件 1: 日线 125 日均线上移                                      │
│  条件 2: 周线 KDJ 的 J 线 < 80                                     │
│  条件 3: 今日最高价 > 昨日最高价（不复权）                        │
│  条件 4: 收盘价上穿 20 日均线（前复权）                           │
│  条件 5: RSI 日线上穿 50                                         │
│  条件 6: 成交量环比增长 ≥ 80%                                    │
│  条件 7: 当日涨跌幅 0% ~ 5%                                      │
│  条件 8: 三个月内最高/最低价比 < 1.5                             │
│  条件 9: 最近 1 年涨幅 < 50%                                     │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔍 条件详细解析

### 条件 1: 日线 125 日均线上移

**含义**: 长期趋势向上

**计算**:
```python
MA125_today = sum(close[-125:]) / 125
MA125_yesterday = sum(close[-126:-1]) / 125

条件：MA125_today > MA125_yesterday
```

**意义**: 
- 125 日 ≈ 半年交易线
- 均线上移表示长期趋势向好
- 过滤掉长期下跌的股票

---

### 条件 2: 周线 KDJ 的 J 线 < 80

**含义**: 周线级别未超买

**计算**:
```python
# KDJ 指标（周线）
RSV = (收盘价 - N 日最低) / (N 日最高 - N 日最低) × 100
K = RSV 的 M1 日移动平均
D = K 的 M2 日移动平均
J = 3K - 2D

条件：J < 80
```

**意义**:
- J > 80 表示超买，可能回调
- 周线级别更可靠
- 避免追高

---

### 条件 3: 今日最高价 > 昨日最高价（不复权）

**含义**: 价格突破昨日高点

**计算**:
```python
条件：High_today > High_yesterday
```

**意义**:
- 短期强势信号
- 突破形态
- 使用不复权价格避免除权干扰

---

### 条件 4: 收盘价上穿 20 日均线（前复权）

**含义**: 短期金叉信号

**计算**:
```python
MA20 = sum(close[-20:]) / 20

条件：Close_today > MA20_today 
  AND Close_yesterday <= MA20_yesterday
```

**意义**:
- 20 日是月线，重要支撑/压力位
- 上穿表示转强
- 使用复权价格保证连续性

---

### 条件 5: RSI 日线上穿 50

**含义**: 动量转强

**计算**:
```python
# RSI(14)
avg_gain = sum(上涨幅度[-14:]) / 14
avg_loss = sum(下跌幅度[-14:]) / 14
RS = avg_gain / avg_loss
RSI = 100 - (100 / (1 + RS))

条件：RSI_today > 50 AND RSI_yesterday <= 50
```

**意义**:
- RSI > 50 表示多头强势
- 上穿 50 是转强信号
- 过滤弱势股

---

### 条件 6: 成交量环比增长 ≥ 80%

**含义**: 放量

**计算**:
```python
volume_ratio = (Volume_today - Volume_yesterday) / Volume_yesterday

条件：volume_ratio >= 0.80
```

**意义**:
- 成交量放大 80% 以上
- 量价配合
- 有资金关注

---

### 条件 7: 当日涨跌幅 0% ~ 5%

**含义**: 温和上涨，非涨停

**计算**:
```python
change_pct = (Close - PreClose) / PreClose × 100

条件：0% <= change_pct <= 5%
```

**意义**:
- 排除涨停股（买不进）
- 排除大跌股
- 温和上涨更可持续

---

### 条件 8: 三个月内最高/最低价比 < 1.5

**含义**: 区间振幅不大

**计算**:
```python
# 3 个月 ≈ 60 个交易日
high_3m = max(high[-60:])
low_3m = min(low[-60:])
range_ratio = high_3m / low_3m

条件：range_ratio < 1.5
```

**意义**:
- 排除暴涨暴跌股
- 走势相对平稳
- 降低风险

---

### 条件 9: 最近 1 年涨幅 < 50%

**含义**: 非大牛股

**计算**:
```python
# 1 年 ≈ 250 个交易日
close_1y_ago = close[-250]
change_1y = (Close_today - close_1y_ago) / close_1y_ago × 100

条件：change_1y < 50%
```

**意义**:
- 排除已大幅上涨的股票
- 寻找补涨机会
- 避免高位接盘

---

## 📊 条件组合逻辑

```
所有条件必须同时满足 (AND 逻辑):

    ┌──────────────────┐
    │  条件 1: MA125↑   │
    │  条件 2: KDJ J<80 │
    │  条件 3: High↑    │
    │  条件 4: 上穿 MA20 │
    │  条件 5: RSI>50   │
    │  条件 6: Vol≥80%  │
    │  条件 7: 0%~5%    │
    │  条件 8: 振幅<1.5 │
    │  条件 9: 年涨<50% │
    └────────┬─────────┘
             │
             ▼
    ┌──────────────────┐
    │  选股结果        │
    │  (全部通过)      │
    └──────────────────┘
```

---

## ⚙️ 配置说明

### 配置文件位置

```
config/strategies.yaml
```

### 配置示例

```yaml
stock_selector:
  # 技术面条件配置
  technical_condition:
    # 条件 1: 日线 125 日均线上移
    ma125_shift_up: true
    
    # 条件 2: 周线 KDJ 的 J 线 < 80
    kdj_j_max: 80.0
    
    # 条件 3: 今日最高价 > 昨日最高价
    high_break_yesterday: true
    
    # 条件 4: 收盘价上穿 20 日均线
    ma20_golden_cross: true
    
    # 条件 5: RSI 日线上穿 50
    rsi_cross_50: true
    
    # 条件 6: 成交量环比增长 ≥ 80%
    volume_growth_min: 0.80
    
    # 条件 7: 当日涨跌幅 0% ~ 5%
    change_pct_min: 0.0
    change_pct_max: 5.0
    
    # 条件 8: 三个月内最高/最低价比 < 1.5
    range_3m_ratio_max: 1.5
    
    # 条件 9: 最近 1 年涨幅 < 50%
    change_1y_max: 50.0
```

### 参数调整建议

| 参数 | 默认值 | 保守 | 激进 |
|------|--------|------|------|
| kdj_j_max | 80 | 70 | 90 |
| volume_growth_min | 0.80 | 1.00 | 0.50 |
| change_pct_max | 5.0 | 3.0 | 8.0 |
| range_3m_ratio_max | 1.5 | 1.3 | 2.0 |
| change_1y_max | 50.0 | 30.0 | 80.0 |

---

## 💻 使用示例

### 基础用法

```python
from strategies.stock_selector import StockSelector

# 配置
config = {
    'technical_condition': {
        'ma125_shift_up': True,
        'kdj_j_max': 80.0,
        'high_break_yesterday': True,
        'ma20_golden_cross': True,
        'rsi_cross_50': True,
        'volume_growth_min': 0.80,
        'change_pct_min': 0.0,
        'change_pct_max': 5.0,
        'range_3m_ratio_max': 1.5,
        'change_1y_max': 50.0,
    },
    'stock_pool': {
        'exclude_st': True,
        'min_market_cap': 50,
    }
}

# 创建选股器
selector = StockSelector(config)
selector.set_api_client(api_client)  # 设置 API 客户端

# 执行选股
stocks = await selector.select_stocks(method='technical', top_n=10)

# 输出结果
for stock in stocks:
    print(f"{stock.symbol} - {stock.name}")
```

### 单独使用技术面选股

```python
# 直接调用技术面选股
candidates = await selector._select_technical()

print(f"通过筛选：{len(candidates)}只股票")
```

### 获取技术指标详情

```python
# 获取某只股票的技术指标
klines = await api_client.get_kline('600519.SH', '1d', 250)
weekly = await api_client.get_kline('600519.SH', '1w', 52)

indicators = selector._calculate_indicators('600519.SH', klines, weekly)

print(f"MA125: {indicators.ma125}")
print(f"KDJ J: {indicators.kdj_j}")
print(f"RSI: {indicators.rsi}")
print(f"成交量增长：{indicators.volume_ratio*100:.1f}%")
```

---

## 🧪 测试

### 运行示例脚本

```bash
cd guojin_auto_trading
python examples/technical_selector_example.py
```

### 测试输出示例

```
============================================================
技术面选股测试 - 9 大条件
============================================================

📋 技术面条件配置:
  1. MA125 上移：True
  2. KDJ J < 80.0
  3. 今日最高 > 昨日最高：True
  4. 收盘上穿 MA20: True
  5. RSI 上穿 50: True
  6. 成交量增长 ≥ 80%
  7. 涨跌幅 0.0% ~ 5.0%
  8. 3 月振幅 < 1.5
  9. 1 年涨幅 < 50.0%

📊 技术指标计算示例:

  600519.SH - 贵州茅台:
    MA125: 1750.23 (前：1748.56) ↑
    MA20: 1780.45
    KDJ: K=65.2, D=60.1, J=75.3
    RSI: 55.8 (前：48.2)
    成交量增长：95.2%
    涨跌幅：2.35%
    3 月振幅：1.32
    1 年涨幅：35.6%

============================================================
技术面选股结果:
============================================================

✅ 通过筛选的股票：3 只
  - 600519.SH 贵州茅台
  - 000858.SZ 五粮液
  - 601318.SH 中国平安
```

---

## 📈 策略优化建议

### 1. 条件权重

当前是"全有或全无"逻辑，可以考虑：
```python
# 评分制：满足条件越多分数越高
score = sum(condition_met) / 9
if score >= 0.8:  # 满足 80% 条件即可
    candidates.append(stock)
```

### 2. 增加基本面过滤

```python
# 先技术面筛选，再基本面排序
technical_stocks = await selector._select_technical()
scored = selector._score_stocks(technical_stocks)
top_stocks = sorted(scored, key=lambda x: x.total_score, reverse=True)[:10]
```

### 3. 行业分散

```python
# 限制每个行业的股票数量
sector_count = {}
for stock in candidates:
    sector = get_sector(stock.symbol)
    if sector_count.get(sector, 0) < 3:
        final_selection.append(stock)
        sector_count[sector] = sector_count.get(sector, 0) + 1
```

### 4. 动态参数

```python
# 根据市场状态调整参数
if market_trend == 'bull':
    config['kdj_j_max'] = 90  # 牛市放宽
elif market_trend == 'bear':
    config['volume_growth_min'] = 1.0  # 熊市要求更高成交量
```

---

## ⚠️ 注意事项

1. **数据质量**: 确保 K 线数据准确、完整
2. **复权处理**: 价格条件用不复权，趋势条件用前复权
3. **交易时间**: 盘中选股注意数据更新频率
4. **停牌股票**: 自动排除停牌股票
5. **新股**: 上市不足 250 天的股票数据不足，自动排除

---

## 📚 参考资料

- 《技术分析基础》- 约翰·墨菲
- 《日本蜡烛图技术》- 史蒂夫·尼森
- [聚宽量化学院](https://www.joinquant.com/view/community/)
- [优矿 Uqer](https://uqer.datayes.com/)

---

**最后更新**: 2026-03-18
