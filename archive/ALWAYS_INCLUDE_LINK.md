# 📊 每次回测结果都包含 GitHub Pages 链接

## 标准化输出格式

---

## 🎯 标准格式

每次回测完成后，输出都会包含以下链接：

```
🔗 查看交互式回测报告
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📱 GitHub Pages 链接:
  https://rickyhanjp.github.io/gold-etf-report/report.html

🔄 强制刷新链接:
  https://rickyhanjp.github.io/gold-etf-report/report.html?t=1711248300

📊 报告包含:
  ✅ 交互式收益对比图表
  ✅ 宏观因子走势分析
  ✅ 信号得分动态展示
  ✅ 完整交易记录
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## 📝 输出示例

### 回测完成输出

```
======================================================================
回测完成!
======================================================================

📊 绩效摘要:
  多因子策略：139.55%
  买入持有：164.88%
  超额收益：-25.33%

======================================================================
🔗 查看交互式回测报告
======================================================================

📱 GitHub Pages 链接:
  https://rickyhanjp.github.io/gold-etf-report/report.html

🔄 强制刷新链接:
  https://rickyhanjp.github.io/gold-etf-report/report.html?t=1711248300

📊 报告包含:
  ✅ 交互式收益对比图表
  ✅ 宏观因子走势分析
  ✅ 信号得分动态展示
  ✅ 完整交易记录

======================================================================
```

---

## 🚀 如何触发

### 方式 1: 运行回测脚本

```bash
cd C:\Users\Administrator\.openclaw\workspace\guojin_auto_trading
python backtest/backtest_github_2023_2026.py
```

输出会自动包含 GitHub Pages 链接。

---

### 方式 2: 一键部署

```bash
run_backtest_and_deploy.bat
```

完成后的输出：

```
✅ 部署完成！

📱 GitHub Pages 链接:
  https://rickyhanjp.github.io/gold-etf-report/report.html

🔄 强制刷新:
  https://rickyhanjp.github.io/gold-etf-report/report.html?t=1711248300

📊 报告包含:
  ✅ 交互式收益对比图表
  ✅ 宏观因子走势分析
  ✅ 完整交易记录
```

---

### 方式 3: 生成客户报告

```bash
python client_feedback_template.py --client "张先生"
```

生成的报告会包含链接：

```markdown
## 查看交互式报告

🔗 https://rickyhanjp.github.io/gold-etf-report/report.html
```

---

## 📧 发送给客户的标准格式

### 微信/短信模板

```
【518880 回测结果】

总收益：139.55%
年化：42.9%
回撤：-3.69%

查看完整图表报告:
https://rickyhanjp.github.io/gold-etf-report/report.html
```

---

### 邮件模板

```
尊敬的客户：

518880 黄金 ETF 多因子策略回测已完成。

核心绩效:
- 总收益率：139.55%
- 年化收益：42.9%
- 最大回撤：-3.69%

📊 查看完整交互式报告（含图表）:
https://rickyhanjp.github.io/gold-etf-report/report.html

报告包含收益对比、宏观因子、交易记录等详细信息。

祝好！
```

---

## 🔗 链接格式说明

### 主链接
```
https://rickyhanjp.github.io/gold-etf-report/report.html
```

**用途**: 标准访问链接

---

### 强制刷新链接
```
https://rickyhanjp.github.io/gold-etf-report/report.html?t=时间戳
```

**用途**: 清除缓存，确保看到最新版本

**示例**:
```
https://rickyhanjp.github.io/gold-etf-report/report.html?t=1711248300
```

---

### CDN 备用链接
```
https://cdn.jsdelivr.net/gh/rickyhanjp/gold-etf-report@main/report.html
```

**用途**: GitHub Pages 无法访问时的备用方案

---

## 📊 报告内容

访问链接后将看到：

### 1. 核心绩效指标
- 总收益率（139.55%）
- 年化收益（42.9%）
- 最大回撤（-3.69%）
- 夏普比率（4.61）
- 交易次数（66 次）

### 2. 交互式图表
- 📈 收益对比图（策略 vs 买入持有）
- 📊 宏观因子图（CPI + PMI）
- 📉 信号得分图（0-12 分）

### 3. 交易记录
- 完整 66 笔交易详情
- 每笔包含日期、操作、价格、得分

### 4. 策略说明
- 趋势过滤逻辑
- 12 个宏观因子
- 动态仓位管理

---

## ✅ 检查清单

每次回测后确认：

- [ ] 输出包含 GitHub Pages 链接
- [ ] 链接格式正确
- [ ] 强制刷新链接可用
- [ ] 报告内容完整
- [ ] 图表正常显示

---

## 📞 快速命令

| 操作 | 命令 | 输出包含链接 |
|------|------|-------------|
| 运行回测 | `python backtest_github_2023_2026.py` | ✅ |
| 一键部署 | `run_backtest_and_deploy.bat` | ✅ |
| 生成报告 | `python client_feedback_template.py` | ✅ |
| 仅部署 | `python auto_deploy_to_github.py` | ✅ |

---

## 📚 相关文件

| 文件 | 说明 |
|------|------|
| [`backtest_github_2023_2026.py`](backtest/backtest_github_2023_2026.py) | 回测脚本（已修改） |
| [`auto_deploy_to_github.py`](auto_deploy_to_github.py) | 自动部署（已修改） |
| [`client_feedback_template.py`](client_feedback_template.py) | 客户报告生成 |

---

## 🎯 总结

**从现在开始，每次回测都会自动包含**:

```
📱 GitHub Pages 链接:
  https://rickyhanjp.github.io/gold-etf-report/report.html
```

**方便你随时点击查看图形化的回测结果！** 📊

---

**更新时间**: 2026-03-24 00:47  
**策略**: 518880 黄金 ETF 多因子策略  
**GitHub**: rickyhanjp/gold-etf-report
