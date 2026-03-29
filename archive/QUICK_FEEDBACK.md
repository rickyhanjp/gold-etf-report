# 📧 客户反馈快速生成指南

## 自动生成包含 GitHub Pages 链接的报告

---

## 🚀 快速使用

### 方式 1: 一键生成（推荐）

```bash
cd C:\Users\Administrator\.openclaw\workspace\guojin_auto_trading
python client_feedback_template.py --all
```

**生成内容**:
- ✅ Markdown 格式报告
- ✅ 纯文本格式报告
- ✅ HTML 邮件格式报告
- ✅ 自动包含 GitHub Pages 链接

---

### 方式 2: 指定客户名称

```bash
python client_feedback_template.py --client "张先生" --message "策略表现优秀，建议配置 10-20% 仓位"
```

---

### 方式 3: 指定格式

```bash
# 仅 Markdown
python client_feedback_template.py --format md

# 仅 HTML 邮件
python client_feedback_template.py --format html

# 仅纯文本
python client_feedback_template.py --format txt
```

---

## 📊 输出示例

### Markdown 格式

```markdown
# 📊 518880 黄金 ETF 多因子策略 - 回测报告

## 核心绩效

| 指标 | 数值 |
|------|------|
| 总收益率 | 139.55% |
| 年化收益 | 42.9% |
| 最大回撤 | -3.69% |
| 夏普比率 | 4.61 |

## 查看交互式报告

**点击这里**: https://rickyhanjp.github.io/gold-etf-report/report.html
```

### HTML 邮件格式

包含：
- 📊 绩效卡片
- 🔗 查看报告按钮
- 📱 响应式设计

---

## 📁 输出位置

报告保存在：
```
C:\Users\Administrator\.openclaw\workspace\guojin_auto_trading\client_reports\
```

文件命名：
```
client_report_20260324_004500.md
client_report_20260324_004500.txt
client_report_20260324_004500.html
```

---

## 🔗 标准链接格式

每次反馈都包含这个链接：

```
https://rickyhanjp.github.io/gold-etf-report/report.html
```

**强制刷新版本**:
```
https://rickyhanjp.github.io/gold-etf-report/report.html?t=时间戳
```

---

## 📝 模板内容

### 包含要素

1. ✅ **核心绩效指标**
   - 总收益率
   - 年化收益
   - 最大回撤
   - 夏普比率
   - 交易次数

2. ✅ **GitHub Pages 链接**
   - 在线访问链接
   - 强制刷新链接
   - 仓库链接

3. ✅ **策略说明**
   - 策略类型
   - 回测期间
   - 调仓频率

4. ✅ **风险提示**
   - 历史业绩不代表未来
   - 投资有风险

---

## 🎯 使用场景

### 场景 1: 微信/短信发送

```bash
python client_feedback_template.py --client "王先生" --format txt
```

复制生成的纯文本，通过微信发送。

---

### 场景 2: 邮件发送

```bash
python client_feedback_template.py --client "李女士" --format html
```

用邮件客户端打开 HTML 文件，或直接复制内容。

---

### 场景 3: 正式报告

```bash
python client_feedback_template.py --client "张总" --format md
```

生成 Markdown 报告，可转换为 PDF。

---

## 📞 快速命令

### 完整流程

```bash
# 1. 运行回测
python backtest/backtest_github_2023_2026.py

# 2. 部署到 GitHub
python auto_deploy_to_github.py --skip-backtest

# 3. 生成客户报告
python client_feedback_template.py --client "客户名称"
```

### 一键完成

创建批处理脚本 `send_to_client.bat`:

```batch
@echo off
echo 生成客户反馈报告...
python client_feedback_template.py --client "%1" --format all
echo.
echo 报告已生成到 client_reports 目录
echo.
echo GitHub Pages 链接:
echo https://rickyhanjp.github.io/gold-etf-report/report.html
echo.
pause
```

使用：
```bash
send_to_client.bat "张先生"
```

---

## 📧 邮件模板

### 主题
```
【回测报告】518880 黄金 ETF 多因子策略 - {客户姓名}
```

### 正文
```
尊敬的{客户姓名}：

您好！

这是您要的 518880 黄金 ETF 多因子策略回测报告。

核心绩效：
- 总收益率：139.55%
- 年化收益：42.9%
- 最大回撤：-3.69%

查看完整报告（含交互式图表）:
https://rickyhanjp.github.io/gold-etf-report/report.html

如有任何问题，请随时联系我。

祝好！
{您的姓名}
```

---

## 💡 最佳实践

### 1. 标准化流程

```
回测 → 部署 → 生成报告 → 发送给客户
```

### 2. 个性化定制

- 添加客户名称
- 添加自定义建议
- 添加联系方式

### 3. 及时更新

- 每次回测后更新 GitHub Pages
- 确保链接指向最新版本
- 使用强制刷新参数

---

## 🔗 快速链接

| 操作 | 链接/命令 |
|------|----------|
| 生成报告 | `python client_feedback_template.py` |
| GitHub Pages | https://rickyhanjp.github.io/gold-etf-report/report.html |
| 强制刷新 | https://rickyhanjp.github.io/gold-etf-report/report.html?t=时间戳 |
| 仓库 | https://github.com/rickyhanjp/gold-etf-report |

---

## 📚 相关文件

| 文件 | 说明 |
|------|------|
| [`client_feedback_template.py`](client_feedback_template.py) | **报告生成脚本** ⭐ |
| [`auto_deploy_to_github.py`](auto_deploy_to_github.py) | 自动部署脚本 |
| [`client_reports/`](client_reports/) | 生成的报告目录 |

---

**每次给客户反馈，只需运行**:

```bash
python client_feedback_template.py --client "客户名称"
```

**自动包含 GitHub Pages 链接**:
# https://rickyhanjp.github.io/gold-etf-report/report.html

---

**更新时间**: 2026-03-24 00:47  
**策略**: 518880 黄金 ETF 多因子策略  
**GitHub**: rickyhanjp/gold-etf-report
