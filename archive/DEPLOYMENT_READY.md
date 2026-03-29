# 📊 518880 黄金 ETF 日线策略回测 - 部署完成

## ✅ 部署状态

**生成时间**: 2026-03-24 13:37  
**状态**: 就绪可部署  
**部署包**: `guojin_auto_trading/deploy_package/`

---

## 📁 部署包内容

```
deploy_package/
├── index.html          (23 KB)  - 交互式 HTML 报告
├── README.md           (3 KB)   - 部署说明
└── .git/                      - Git 仓库（已初始化）
```

---

## 🌐 部署方式

### 方式 1: Netlify Drop (最简单，推荐)

**时间**: 2 分钟

1. 打开浏览器访问：**https://app.netlify.com/drop**
2. 打开文件夹：`C:\Users\Administrator\.openclaw\workspace\guojin_auto_trading\deploy_package`
3. 将整个 `deploy_package` 文件夹拖到 Netlify 页面
4. 等待 30 秒
5. 获得在线链接，例如：`https://your-site-name.netlify.app`

**优点**:
- ✅ 无需注册（可先部署后注册）
- ✅ 免费 HTTPS
- ✅ 自动 CDN
- ✅ 支持自定义域名

---

### 方式 2: GitHub Pages

**时间**: 5 分钟

1. 创建 GitHub 仓库（例如：`gold-etf-report`）
2. 命令行操作：
   ```bash
   cd deploy_package
   git remote add origin https://github.com/YOUR_USERNAME/gold-etf-report.git
   git branch -M gh-pages
   git push -u origin gh-pages
   ```
3. 在 GitHub 仓库 Settings → Pages 启用 GitHub Pages
4. 选择 `gh-pages` 分支
5. 等待 1-2 分钟
6. 访问：`https://YOUR_USERNAME.github.io/gold-etf-report/`

**优点**:
- ✅ 完全免费
- ✅ 自定义域名
- ✅ 与代码仓库集成

---

### 方式 3: Vercel

**时间**: 3 分钟

1. 访问：**https://vercel.com/new**
2. 登录 GitHub 账号
3. Import 项目（选择 `deploy_package` 目录）
4. 点击 Deploy
5. 获得链接：`https://your-project.vercel.app`

**优点**:
- ✅ 自动 HTTPS
- ✅ 全球 CDN
- ✅ 自动预览部署

---

## 📊 回测结果摘要

| 指标 | 数值 |
|------|------|
| 总收益率 | **93.94%** |
| 年化收益率 | **25.00%** |
| 最大回撤 | -19.84% |
| 夏普比率 | 1.55 |
| 胜率 | 100% |
| 交易次数 | 14 |
| 最终资产 | ¥193,938.32 |

---

## 📝 关键交易

**最佳操作**: 2026-01-29 卖出 10100 股 @ 11.904 元
- 精准逃顶（历史最高点）
- 实现盈利约 74,000 元

**当前持仓**: 10,600 股（成本约 10.5 元）
- 2026-03 月回调期间分批买入
- 等待下一轮上涨

---

## 🔗 本地预览

直接打开 HTML 报告：
```
C:\Users\Administrator\.openclaw\workspace\guojin_auto_trading\deploy_package\index.html
```

或使用本地服务器：
```bash
cd deploy_package
python -m http.server 8080
# 访问 http://localhost:8080
```

---

## 📈 数据源

- **数据来源**: 新浪财经 API（通过 AkShare）
- **数据量**: 777 条真实日线数据
- **日期范围**: 2023-03-24 至 2026-03-24
- **标的**: 518880 (华安黄金易 ETF)

---

## 🛠️ 技术栈

- **回测引擎**: Python + Pandas
- **数据获取**: AkShare
- **图表**: Chart.js
- **报告**: HTML5 + CSS3

---

## 📞 联系

**策略版本**: V1.0.0  
**生成工具**: OpenClaw  
**作者**: 量化策略组

---

*下次更新时间：下次运行 `python run_3year_backtest.py` 后*
