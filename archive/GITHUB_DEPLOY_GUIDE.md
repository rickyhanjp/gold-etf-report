# 🚀 GitHub Pages 自动部署指南

## 当前状态

✅ Git 已安装 (v2.53.0)
✅ 所有文件已准备就绪
✅ 部署包位置：`deploy_package/`

---

## ⚠️ 重要提示

由于需要 GitHub 账号认证，**无法完全自动部署**。

但我可以帮你完成**90% 的准备工作**，你只需：
1. 登录 GitHub
2. 授权一次
3. 复制粘贴命令

---

## 📋 手动部署步骤（5 分钟）

### 第 1 步：创建 GitHub 仓库

1. 打开：https://github.com/new
2.  Repository name: `gold-etf-backtest`
3.  设为 **Public**（公共）
4.  点击 **Create repository**

### 第 2 步：上传文件

**复制以下命令到命令行执行：**

```bash
# 进入部署包目录
cd C:\Users\Administrator\.openclaw\workspace\guojin_auto_trading\deploy_package

# 初始化 git
git init

# 添加所有文件
git add .

# 提交
git commit -m "Initial commit: 518880 gold ETF backtest report"

# 设置分支
git branch -M main

# 添加远程仓库（替换 YOUR_USERNAME 为你的 GitHub 用户名）
git remote add origin https://github.com/YOUR_USERNAME/gold-etf-backtest.git

# 推送
git push -u origin main
```

### 第 3 步：启用 GitHub Pages

1. 打开你的仓库页面
2. 点击 **Settings**
3. 左侧菜单点击 **Pages**
4. Source 选择 **main**
5. 点击 **Save**

### 第 4 步：等待部署（1-2 分钟）

GitHub 会自动构建，完成后显示：

```
Your site is live at https://YOUR_USERNAME.github.io/gold-etf-backtest/
```

### 第 5 步：访问报告

```
https://YOUR_USERNAME.github.io/gold-etf-backtest/report.html
```

---

## 🔧 自动部署脚本（如果已登录 GitHub）

如果你已经在命令行登录了 GitHub，可以运行以下脚本：

```bash
# 创建并运行自动部署脚本
cd C:\Users\Administrator\.openclaw\workspace\guojin_auto_trading
python setup_github_deploy.py
```

---

## 📊 预期效果

部署完成后，你将获得：

- ✅ 永久在线链接
- ✅ 免费托管（GitHub Pages）
- ✅ 自动 HTTPS
- ✅ 支持自定义域名
- ✅ 全球 CDN 加速

---

## 🆘 常见问题

### Q: 推送失败，提示认证错误？
**A:** 使用 Personal Access Token 代替密码：
1. GitHub → Settings → Developer settings → Personal access tokens
2. 生成 token（勾选 repo 权限）
3. 推送时使用 token 作为密码

### Q: Pages 不显示？
**A:** 等待 2-5 分钟，GitHub 需要时间构建

### Q: 页面 404？
**A:** 确保：
- 仓库是 Public
- Pages 已启用
- 等待构建完成

---

## 💡 更简单的方式

如果命令行太复杂，推荐使用：

**Netlify Drop（最简单）**
- 网址：https://app.netlify.com/drop
- 操作：拖拽文件夹即可
- 时间：30 秒
- 无需命令行

---

*文档创建时间：2026-03-22*
