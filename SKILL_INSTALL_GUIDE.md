# Skill 安装指南 - 回测分析可视化

## ✅ 已完成的可视化工作

我们已经使用 Python + Matplotlib 生成了完整的回测图表：

### 已生成图表（7 张）

```
backtest/
├── 07_dashboard.png          ⭐ 综合仪表板
├── 01_cumulative_return.png  累计收益对比
├── 02_portfolio_value.png    组合价值对比
├── 03_drawdown.png           回撤对比
├── 04_position_ratio.png     仓位变化
├── 05_annual_return.png      年度收益
└── 06_score_trend.png        因子得分
```

**查看方式：**
```bash
# Windows
start backtest\07_dashboard.png
```

---

## 🔧 推荐安装的 Skill

如需更强大的可视化能力，建议安装以下 Skill：

### 核心可视化（3 个）

| Skill | 用途 | 优先级 |
|-------|------|--------|
| **sci-plotter** | 科研级数据图表 | ⭐⭐⭐⭐⭐ |
| **stock-plot** | 股票 K 线/技术指标 | ⭐⭐⭐⭐⭐ |
| **dashboard-builder** | 交互式仪表盘 | ⭐⭐⭐⭐ |

### 数据处理（2 个）

| Skill | 用途 | 优先级 |
|-------|------|--------|
| **excel-parser** | CSV/Excel 读取 | ⭐⭐⭐⭐⭐ |
| **stats-analyzer** | 统计分析 | ⭐⭐⭐⭐ |

### 可选增强（2 个）

| Skill | 用途 | 优先级 |
|-------|------|--------|
| mermaid-chart | 流程图/架构图 | ⭐⭐⭐ |
| report-generator | 自动报告 | ⭐⭐⭐ |

---

## 📦 安装命令

```bash
# 方式 1：使用 clawhub（推荐）
clawhub install sci-plotter
clawhub install stock-plot
clawhub install dashboard-builder
clawhub install excel-parser
clawhub install stats-analyzer

# 方式 2：使用 OpenClaw
openclaw skill install sci-plotter
openclaw skill install stock-plot
openclaw skill install dashboard-builder
openclaw skill install excel-parser
openclaw skill install stats-analyzer

# 方式 3：批量安装
clawhub install sci-plotter stock-plot dashboard-builder excel-parser stats-analyzer
```

---

## 🎯 安装后用途

### sci-plotter
```python
# 生成更专业的收益对比图
import sci_plotter as sp
sp.line_chart(data, title="累计收益对比")
```

### stock-plot
```python
# 518880 K 线图 + 均线
import stock_plot as st
st.kline_chart("518880", indicators=["MA50", "MA200"])
```

### dashboard-builder
```python
# 交互式仪表板
import dashboard_builder as db
db.create_dashboard([chart1, chart2, chart3])
```

---

## 📊 当前可用方案

如果暂时不安装 Skill，我们已有完整的 Python 可视化方案：

### 查看现有图表
```bash
# 打开综合仪表板
start backtest\07_dashboard.png

# 打开所有图表
explorer backtest\*.png
```

### 重新生成图表
```bash
cd guojin_auto_trading
python visualization/plot_charts.py
```

### 查看回测报告
```bash
# Markdown 报告
cat backtest/518880_final_report.md

# 图表说明
cat backtest/CHARTS_README.md
```

---

## 💡 建议

### 短期（现在）
✅ **使用现有 Python 图表** - 已生成 7 张专业图表
- 质量高（300 DPI）
- 信息完整
- 可直接用于汇报

### 中期（1-2 周）
🔧 **安装核心 Skill**
- sci-plotter：更专业的统计图表
- stock-plot：K 线图和技术指标
- excel-parser：方便数据读取

### 长期（1 月+）
🚀 **完整 Skill 套装**
- dashboard-builder：交互式展示
- report-generator：自动报告
- mermaid-chart：策略流程图

---

## 📈 现有图表预览

### 07_dashboard.png - 综合仪表板

包含：
- ✅ 关键指标（总收益、年化、回撤）
- ✅ 累计收益对比曲线
- ✅ 仓位变化图
- ✅ 因子得分趋势

### 01_cumulative_return.png - 收益对比

显示：
- ✅ 策略 vs 买入持有
- ✅ 超额收益区域
- ✅ 时间序列（2021-2025）

---

## 📞 需要帮助？

如需安装 Skill 或生成更多图表，请告诉我！

---

*文档创建时间：2026-03-22*
*当前状态：7 张图表已生成，可直接使用*
