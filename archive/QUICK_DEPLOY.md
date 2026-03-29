# 🚀 快速部署指南

## 当前状态

✅ 回测完成  
✅ HTML 报告已生成  
✅ 部署包已准备  
✅ Git 仓库已初始化  

**部署包位置**: `C:\Users\Administrator\.openclaw\workspace\guojin_auto_trading\deploy_package`

---

## 方式 1: Netlify Drop (推荐，2 分钟)

### 步骤：

1. **打开 Netlify Drop**
   - 浏览器访问：**https://app.netlify.com/drop**

2. **拖拽部署**
   - 打开文件夹：`deploy_package`
   - 将整个文件夹拖到 Netlify 页面

3. **获得链接**
   - 等待 30 秒
   - 获得在线链接，例如：`https://amazing-site-12345.netlify.app`

4. **（可选）注册账号**
   - 可以稍后注册账号绑定链接
   - 支持自定义域名

### 优点：
- ✅ 无需注册即可部署
- ✅ 免费 HTTPS
- ✅ 全球 CDN
- ✅ 支持自定义域名

---

## 方式 2: GitHub Pages (5 分钟)

### 前提条件：
- 有 GitHub 账号
- 已安装 Git

### 步骤：

1. **创建 GitHub 仓库**
   - 访问：https://github.com/new
   - 仓库名：`gold-etf-report`
   - 设为 Public
   - 点击 "Create repository"

2. **推送部署包**
   ```bash
   cd C:\Users\Administrator\.openclaw\workspace\guojin_auto_trading\deploy_package
   git remote add origin https://github.com/YOUR_USERNAME/gold-etf-report.git
   git branch -M gh-pages
   git push -u origin gh-pages
   ```

3. **启用 GitHub Pages**
   - 在 GitHub 仓库页面，点击 **Settings**
   - 左侧菜单选择 **Pages**
   - Source 选择 `gh-pages` 分支
   - 点击 **Save**

4. **等待部署**
   - 等待 1-2 分钟
   - 刷新页面，看到链接：`https://YOUR_USERNAME.github.io/gold-etf-report/`

### 优点：
- ✅ 完全免费
- ✅ 永久存储
- ✅ 支持自定义域名
- ✅ 与 GitHub 集成

---

## 方式 3: Vercel (3 分钟)

### 步骤：

1. **访问 Vercel**
   - https://vercel.com/new

2. **登录 GitHub**
   - 使用 GitHub 账号登录

3. **导入项目**
   - 点击 "Import Git Repository"
   - 选择刚才创建的 `gold-etf-report` 仓库

4. **部署**
   - 点击 "Deploy"
   - 等待 1 分钟
   - 获得链接：`https://gold-etf-report.vercel.app`

### 优点：
- ✅ 自动 HTTPS
- ✅ 全球 CDN
- ✅ 自动预览部署
- ✅ 性能优秀

---

## 本地预览

### 直接打开：
```
C:\Users\Administrator\.openclaw\workspace\guojin_auto_trading\deploy_package\index.html
```

### 或使用本地服务器：
```bash
cd C:\Users\Administrator\.openclaw\workspace\guojin_auto_trading\deploy_package
python -m http.server 8080
# 访问 http://localhost:8080
```

---

## 回测结果

| 指标 | 数值 |
|------|------|
| **总收益率** | **93.94%** |
| **年化收益率** | **25.00%** |
| **最大回撤** | -19.84% |
| **夏普比率** | 1.55 |
| **胜率** | 100% |
| **最终资产** | ¥193,938.32 |

---

## 文件清单

```
deploy_package/
├── index.html          (24 KB)  ← 主报告页面
├── README.md           (4 KB)   ← 说明文档
├── .gitignore          (35 B)   ← Git 配置
└── .git/                      ← Git 仓库
```

---

## 下次更新

运行以下命令更新回测报告：

```bash
cd C:\Users\Administrator\.openclaw\workspace\guojin_auto_trading
python run_3year_backtest.py
python deploy_to_github.py
```

然后重新部署 `deploy_package` 文件夹。

---

## 需要帮助？

查看完整文档：
- `DEPLOYMENT_READY.md` - 详细部署说明
- `README.md` - 回测报告摘要
- `backtest/output/518880_3year_report.md` - Markdown 报告

---

*生成时间：2026-03-24 13:38*
