# 📊 518880 黄金 ETF - 最新回测结果

**更新时间**: 2026-03-24 00:54

---

## 🎯 核心绩效

| 指标 | 数值 |
|------|------|
| **总收益率** | 139.55% |
| **年化收益** | 42.9% |
| **最大回撤** | -3.69% |
| **夏普比率** | 4.61 |
| **交易次数** | 66 次 |

---

## 🔗 查看交互式报告

### 📱 GitHub Pages（推荐）

**[👉 点击这里查看完整图形化报告](https://rickyhanjp.github.io/gold-etf-report/report.html)**

或直接复制链接到浏览器：
```
https://rickyhanjp.github.io/gold-etf-report/report.html
```

---

### 🔄 强制刷新

如果页面未更新，请点击：

**[🔃 强制刷新查看最新版本](https://rickyhanjp.github.io/gold-etf-report/report.html?t=1711248840)**

---

## 📊 报告包含内容

- ✅ **交互式收益对比图表**（策略 vs 买入持有）
- ✅ **宏观因子走势分析**（CPI + PMI）
- ✅ **信号得分动态展示**（0-12 分）
- ✅ **完整交易记录**（66 笔交易详情）
- ✅ **策略逻辑说明**（趋势过滤 + 12 因子）

---

## 📈 快速链接

| 功能 | 链接 |
|------|------|
| 📊 **查看报告** | [https://rickyhanjp.github.io/gold-etf-report/report.html](https://rickyhanjp.github.io/gold-etf-report/report.html) |
| 🔄 **强制刷新** | [点击刷新](https://rickyhanjp.github.io/gold-etf-report/report.html?t=1711248840) |
| 📦 **GitHub 仓库** | [https://github.com/rickyhanjp/gold-etf-report](https://github.com/rickyhanjp/gold-etf-report) |
| ⚙️ **部署状态** | [https://github.com/rickyhanjp/gold-etf-report/actions](https://github.com/rickyhanjp/gold-etf-report/actions) |

---

## 🚀 快速命令

### 运行回测
```bash
python backtest/backtest_github_2023_2026.py
```

### 部署到 GitHub
```bash
python auto_deploy_to_github.py --skip-backtest
```

### 生成客户报告
```bash
python client_feedback_template.py --client "客户名称"
```

---

## 📝 策略说明

### 策略类型
趋势过滤 + 12 宏观因子打分

### 核心逻辑
1. **趋势过滤**: 价格站上 50 日/200 日均线
2. **因子打分**: 12 个宏观因子（0-12 分）
3. **动态仓位**: 0-4 分空仓，5-7 分半仓，8-12 分满仓

### 调仓频率
月度（每月初根据宏观数据调整）

---

## 💡 结论

### 优势
- ✅ 宏观驱动，逻辑透明
- ✅ 风险控制优秀（回撤 < 4%）
- ✅ 适合震荡市/熊市

### 局限
- ⚠️ 单边牛市可能跑输买入持有
- ⚠️ 宏观数据有滞后性

---

## 📞 联系方式

如有任何问题，请随时联系：

- 📱 微信：[您的微信]
- 📧 邮箱：[您的邮箱]
- 💬 电话：[您的电话]

---

**策略版本**: V1.0.0  
**数据源**: 东方财富 mx-data  
**托管平台**: GitHub Pages

---

⚠️ **风险提示**: 历史业绩不代表未来收益 | 投资有风险，入市需谨慎

---

**[⬆️ 返回顶部](#-518880-黄金-etf---最新回测结果)** | **[📊 查看报告](https://rickyhanjp.github.io/gold-etf-report/report.html)**
