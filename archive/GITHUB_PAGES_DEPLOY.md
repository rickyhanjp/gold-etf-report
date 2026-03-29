# 🚀 GitHub Pages 部署指南

## 📊 518880 黄金 ETF 回测报告

---

## 🎯 快速部署（3 步完成）

### 步骤 1: 创建 GitHub 仓库

1. 登录 GitHub: https://github.com
2. 点击右上角 **+** → **New repository**
3. 填写仓库信息：
   - **Repository name**: `518880-backtest` (或你喜欢的名字)
   - **Description**: 518880 黄金 ETF 多因子策略回测报告
   - **Visibility**: Public (公开) 或 Private (私有)
   - ✅ 勾选 **Add a README file**
4. 点击 **Create repository**

---

### 步骤 2: 上传文件

#### 方式 A: 网页上传（推荐新手）

1. 进入刚创建的仓库
2. 点击 **Add file** → **Upload files**
3. 将以下文件拖拽到上传区域：
   ```
   backtest/index.html          ← 主要报告页面
   ```
4. 填写 Commit message: `Add backtest report`
5. 点击 **Commit changes**

#### 方式 B: Git 命令行

```bash
# 克隆仓库
git clone https://github.com/YOUR_USERNAME/518880-backtest.git
cd 518880-backtest

# 复制报告文件
cp /path/to/guojin_auto_trading/backtest/index.html .

# 提交并推送
git add index.html
git commit -m "Add backtest report"
git push origin main
```

---

### 步骤 3: 启用 GitHub Pages

1. 进入仓库 → 点击 **Settings** (设置)
2. 左侧菜单找到 **Pages**
3. 在 **Build and deployment** 下：
   - **Source**: Deploy from a branch
   - **Branch**: 选择 `main` (或 `master`)
   - **Folder**: 选择 `/ (root)`
4. 点击 **Save**

---

## 🌐 访问你的报告

等待 1-2 分钟部署完成后，访问：

```
https://YOUR_USERNAME.github.io/518880-backtest/
```

或

```
https://YOUR_USERNAME.github.io/518880-backtest/index.html
```

### 示例链接

如果你的 GitHub 用户名是 `zhangsan`：

```
https://zhangsan.github.io/518880-backtest/
```

---

## 📱 分享链接

复制以下格式的链接发送给他人：

```
https://<你的 GitHub 用户名>.github.io/518880-backtest/
```

**特点：**
- ✅ 全球可访问（需要网络）
- ✅ 免费托管
- ✅ 自动 HTTPS
- ✅ 支持自定义域名

---

## 🎨 自定义域名（可选）

如果想使用自己的域名：

1. 进入仓库 **Settings** → **Pages**
2. 在 **Custom domain** 输入你的域名
3. 配置 DNS：
   ```
   类型：CNAME
   名称：www
   值：YOUR_USERNAME.github.io
   ```

---

## 📊 报告内容

访问后将看到：

- 🎯 **核心绩效指标**: 收益率、回撤、夏普比率等
- 📈 **交互式图表**: Plotly 驱动，可悬停查看数据
- 📝 **交易记录**: 完整 66 笔交易
- 🔍 **宏观因子**: CPI、PMI 走势
- ⚙️ **策略逻辑**: 12 因子打分系统

---

## ⚠️ 常见问题

### Q: 页面显示 404？
**A**: 等待 1-2 分钟，GitHub Pages 需要时间构建

### Q: 图表不显示？
**A**: 检查网络连接，Plotly 需要加载 CDN 资源

### Q: 如何更新报告？
**A**: 重新上传 `index.html` 文件，GitHub 会自动更新

### Q: 可以设置为私有吗？
**A**: 
- GitHub Free: 公开仓库才能使用 Pages
- GitHub Pro: 私有仓库也可使用 Pages

---

## 🔗 相关文件

| 文件 | 说明 |
|------|------|
| [`backtest/index.html`](backtest/index.html) | GitHub Pages 主页面 ⭐ |
| [`GITHUB_BACKTEST_REPORT_2023_2026.md`](GITHUB_BACKTEST_REPORT_2023_2026.md) | Markdown 报告 |
| [`BACKTEST_REPORT_LINK.md`](BACKTEST_REPORT_LINK.md) | 本地访问指南 |

---

## 📞 需要帮助？

如果部署遇到问题：

1. 检查 GitHub Pages 状态：https://www.githubstatus.com/
2. 查看 GitHub Pages 文档：https://docs.github.com/en/pages
3. 检查仓库设置是否正确

---

**祝你部署成功！** 🎉
