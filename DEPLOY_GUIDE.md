# GitHub Pages 部署指南

## 快速部署（推荐）

### 方法 1：使用 GitHub CLI（最简单）

1. 安装 GitHub CLI（如果未安装）：
   ```bash
   winget install GitHub.cli
   ```

2. 登录 GitHub：
   ```bash
   gh auth login
   ```

3. 创建/更新仓库并部署：
   ```bash
   cd deploy_package
   gh repo create gold-etf-report --public --source=. --remote=origin --push
   ```

4. 启用 GitHub Pages：
   - 访问 https://github.com/rickyhanjp/gold-etf-report/settings/pages
   - Source 选择 "Deploy from a branch"
   - Branch 选择 "main"，文件夹选择 "/ (root)"
   - 点击 Save

5. 访问链接：
   ```
   https://rickyhanjp.github.io/gold-etf-report/report.html
   ```

### 方法 2：手动推送

1. 初始化 Git 仓库：
   ```bash
   cd deploy_package
   git init
   git add .
   git commit -m "Deploy ETF strategy report"
   ```

2. 添加远程仓库（需要先创建仓库）：
   ```bash
   git remote add origin https://github.com/rickyhanjp/gold-etf-report.git
   git branch -M main
   git push -u origin main
   ```

3. 启用 GitHub Pages（同上）

### 方法 3：使用 GitHub Token 自动部署

1. 获取 GitHub Personal Access Token：
   - 访问 https://github.com/settings/tokens
   - 生成新 Token，勾选 `repo` 权限

2. 创建 .env 文件：
   ```
   GITHUB_TOKEN=你的_token_在这里
   GITHUB_OWNER=rickyhanjp
   GITHUB_REPO=gold-etf-report
   GITHUB_BRANCH=main
   ```

3. 运行自动部署脚本：
   ```bash
   cd ../guojin_auto_trading
   python auto_deploy_to_github.py --skip-backtest
   ```

## 钉钉链接格式

部署完成后，钉钉可点击的链接格式：

```
https://rickyhanjp.github.io/gold-etf-report/report.html
```

在钉钉中直接发送此链接即可自动识别为可点击超链接。

## 报告文件

- `report.html` - 完整交互式回测报告（主页面）
- `index.html` - 自动跳转到 report.html
- 其他 HTML 文件 - 分项图表（收益曲线、回撤分析等）

## 更新报告

每次策略更新后：
1. 重新生成报告文件
2. 复制到 deploy_package 目录
3. 执行 git push 或运行自动部署脚本
