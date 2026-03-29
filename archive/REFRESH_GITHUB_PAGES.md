# 🔧 GitHub Pages 刷新指南

## 🚨 页面没有更新？按以下步骤解决！

---

## ⚡ 快速解决（5 个方法）

### 方法 1: 强制刷新浏览器（最简单）

**Windows:**
```
Ctrl + Shift + R
```
或
```
Ctrl + F5
```

**Mac:**
```
Cmd + Shift + R
```

---

### 方法 2: 清除浏览器缓存

**Chrome/Edge:**
1. 按 `F12` 打开开发者工具
2. 右键点击刷新按钮
3. 选择 **"清空缓存并硬性重新加载"**

**Firefox:**
1. 按 `Ctrl + Shift + Delete`
2. 选择 "缓存"
3. 点击 "立即清除"

---

### 方法 3: 添加时间戳参数（推荐）

在 URL 后面添加时间戳强制刷新：

```
https://rickyhanjp.github.io/gold-etf-report/report.html?t=202603240041
```

或

```
https://rickyhanjp.github.io/gold-etf-report/report.html?v=2
```

**点击这里强制刷新**: 
https://rickyhanjp.github.io/gold-etf-report/report.html?t=202603240041

---

### 方法 4: 检查 GitHub Pages 部署状态

1. **进入仓库 Actions**
   ```
   https://github.com/rickyhanjp/gold-etf-report/actions
   ```

2. **查看最新部署**
   - 找到最近的 workflow run
   - 确认状态是 ✅ **Success**
   - 如果是 ❌ **Failed**，点击查看详情

3. **如果部署失败**
   - 重新上传 `report.html`
   - 或手动触发部署

---

### 方法 5: 使用隐私模式/无痕模式

**Chrome/Edge:**
```
Ctrl + Shift + N
```

**Firefox:**
```
Ctrl + Shift + P
```

然后访问：
```
https://rickyhanjp.github.io/gold-etf-report/report.html
```

---

## 🔍 排查清单

### ✅ 检查文件是否上传成功

1. 进入仓库：https://github.com/rickyhanjp/gold-etf-report
2. 查看 `report.html` 文件
3. 确认 **Last commit** 是最新的（2026-03-24）
4. 查看文件大小应该是 **20-25 KB**

### ✅ 检查 Pages 设置

1. Settings → Pages
2. 确认设置：
   - Source: **Deploy from a branch**
   - Branch: **main** (或 master)
   - Folder: **/ (root)**
3. 确认顶部显示：
   ```
   Your site is live at https://rickyhanjp.github.io/gold-etf-report/
   ```

### ✅ 检查文件内容

打开仓库中的 `report.html`，确认包含：
```html
<title>518880 黄金 ETF - 多因子策略回测报告</title>
```

如果还是旧内容，说明文件没有更新成功。

---

## 🔄 重新上传文件

### 方式 1: 网页重新上传

1. 进入仓库：https://github.com/rickyhanjp/gold-etf-report
2. 点击 `report.html` 文件
3. 点击右上角 **...** → **Delete file**
4. Commit deletion
5. 重新上传新的 `report.html`
6. Commit changes

### 方式 2: 使用 Git 命令行

```bash
# 克隆仓库
git clone https://github.com/rickyhanjp/gold-etf-report.git
cd gold-etf-report

# 备份旧文件
cp report.html report.html.backup

# 复制新文件
cp /path/to/backtest/report.html .

# 强制提交
git add report.html
git commit -m "Force update report with new data"
git push -f origin main
```

---

## 🌐 CDN 缓存问题

GitHub Pages 使用 CDN，可能需要更长时间更新：

### 等待时间
- **通常**: 2-5 分钟
- **最长**: 10-15 分钟

### 加速方法
1. 访问：https://www.jsdelivr.com/tools/purge
2. 输入你的页面 URL
3. 点击 **Purge**

---

## 📱 使用不同设备测试

### 手机访问
```
https://rickyhanjp.github.io/gold-etf-report/report.html
```

### 使用其他浏览器
- Chrome
- Firefox
- Edge
- Safari

---

## 🔗 备用访问链接

### 链接 1: 带时间戳
```
https://rickyhanjp.github.io/gold-etf-report/report.html?t=1711248061
```

### 链接 2: 使用 index.html
如果仓库有 `index.html`：
```
https://rickyhanjp.github.io/gold-etf-report/index.html
```

### 链接 3: 使用 jsDelivr CDN
```
https://cdn.jsdelivr.net/gh/rickyhanjp/gold-etf-report@main/report.html
```

---

## 📊 验证新内容

新报告应该包含以下内容：

### 标题
```
518880 黄金 ETF - 多因子策略回测报告
```

### 回测期间
```
2023-01-01 至 2026-03-31
```

### 核心绩效
- 总收益率：**139.55%**
- 年化收益：**42.9%**
- 最大回撤：**-3.69%**
- 夏普比率：**4.61**
- 交易次数：**66 次**

### 图表
- 📈 收益对比图（蓝色 vs 灰色）
- 📊 宏观因子图（CPI + PMI）
- 📉 信号得分图（紫色）

---

## ⚠️ 如果还是不行

### 检查 GitHub 状态
```
https://www.githubstatus.com/
```

### 查看 GitHub Pages 文档
```
https://docs.github.com/en/pages
```

### 联系支持
如果超过 30 分钟还没更新，可能是 GitHub 问题。

---

## 📞 快速链接汇总

| 操作 | 链接 |
|------|------|
| 你的仓库 | https://github.com/rickyhanjp/gold-etf-report |
| **强制刷新** | **https://rickyhanjp.github.io/gold-etf-report/report.html?t=202603240041** |
| Actions 状态 | https://github.com/rickyhanjp/gold-etf-report/actions |
| Pages 设置 | https://github.com/rickyhanjp/gold-etf-report/settings/pages |
| CDN 链接 | https://cdn.jsdelivr.net/gh/rickyhanjp/gold-etf-report@main/report.html |
| GitHub 状态 | https://www.githubstatus.com/ |

---

## ✅ 最终检查清单

- [ ] 强制刷新浏览器（Ctrl+Shift+R）
- [ ] 清除浏览器缓存
- [ ] 使用时间戳链接访问
- [ ] 检查仓库中文件已更新
- [ ] 检查 Pages 部署状态（Actions）
- [ ] 等待 5-10 分钟
- [ ] 使用无痕模式测试
- [ ] 使用手机/其他浏览器测试
- [ ] 检查 GitHub 状态

---

**立即尝试强制刷新**: 
# https://rickyhanjp.github.io/gold-etf-report/report.html?t=202603240041

---

**更新时间**: 2026-03-24 00:41  
**用户名**: rickyhanjp  
**仓库名**: gold-etf-report
