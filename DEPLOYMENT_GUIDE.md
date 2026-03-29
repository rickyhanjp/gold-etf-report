# 518880 黄金 ETF 回测报告 - 部署指南

## 📁 待部署文件

```
guojin_auto_trading/backtest/
├── report.html              # 主页面
├── 01_cumulative_return.png
├── 02_portfolio_value.png
├── 03_drawdown.png
├── 04_position_ratio.png
├── 05_annual_return.png
├── 06_score_trend.png
└── 07_dashboard.png
```

---

## 🚀 部署方案

### 方案 1：本地预览（最快）

**用途：** 本地测试和演示

```bash
cd guojin_auto_trading
python server.py
```

**访问地址：** http://localhost:8080/report.html

---

### 方案 2：GitHub Pages（免费推荐）⭐

**用途：** 永久在线，免费托管

**步骤：**

1. **创建 GitHub 仓库**
   ```bash
   # 创建新仓库
   gh repo create gold-etf-backtest --public
   ```

2. **上传文件**
   ```bash
   cd guojin_auto_trading/backtest
   git init
   git add *.html *.png
   git commit -m "Initial commit: 518880 backtest report"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/gold-etf-backtest.git
   git push -u origin main
   ```

3. **启用 GitHub Pages**
   - 打开仓库 Settings → Pages
   - Source 选择 `main branch`
   - 等待 1-2 分钟

4. **访问地址**
   ```
   https://YOUR_USERNAME.github.io/gold-etf-backtest/report.html
   ```

**优点：**
- ✅ 完全免费
- ✅ 永久在线
- ✅ 支持自定义域名
- ✅ 自动 HTTPS

---

### 方案 3：Vercel 部署（推荐）⭐⭐

**用途：** 企业级部署，自动 CI/CD

**步骤：**

1. **安装 Vercel CLI**
   ```bash
   npm install -g vercel
   ```

2. **部署**
   ```bash
   cd guojin_auto_trading/backtest
   vercel --prod
   ```

3. **按提示操作**
   - 登录 Vercel 账号
   - 确认项目设置
   - 等待部署完成

4. **访问地址**
   ```
   https://gold-etf-backtest.vercel.app/report.html
   ```

**优点：**
- ✅ 免费套餐够用
- ✅ 全球 CDN
- ✅ 自动 HTTPS
- ✅ 支持自定义域名

---

### 方案 4：Netlify（简单）⭐

**用途：** 拖拽部署，最简单

**步骤：**

1. **打开 Netlify**
   - 访问：https://app.netlify.com/drop

2. **拖拽文件夹**
   - 将 `backtest` 文件夹拖到上传区域

3. **访问地址**
   ```
   https://random-name.netlify.app/report.html
   ```

**优点：**
- ✅ 无需命令行
- ✅ 拖拽即可
- ✅ 自动 HTTPS

---

### 方案 5：公司/个人服务器

**用途：** 内部部署，可控性强

**步骤：**

1. **上传文件到服务器**
   ```bash
   scp -r backtest/* user@your-server:/var/www/html/backtest/
   ```

2. **配置 Nginx**
   ```nginx
   server {
       listen 80;
       server_name your-domain.com;
       
       location /backtest/ {
           root /var/www/html;
           index report.html;
       }
   }
   ```

3. **重启 Nginx**
   ```bash
   sudo systemctl restart nginx
   ```

4. **访问地址**
   ```
   http://your-domain.com/backtest/report.html
   ```

---

### 方案 6：阿里云 OSS（静态网站）

**用途：** 企业级，高可用

**步骤：**

1. **创建 OSS Bucket**
   - 登录阿里云控制台
   - 创建 Bucket，选择"静态网站托管"

2. **上传文件**
   ```bash
   ossutil cp -r backtest/ oss://your-bucket/backtest/
   ```

3. **设置权限**
   - Bucket 权限设为"公共读"

4. **访问地址**
   ```
   http://your-bucket.oss-cn-hangzhou.aliyuncs.com/backtest/report.html
   ```

---

## 🎯 推荐方案对比

| 方案 | 难度 | 费用 | 速度 | 推荐度 |
|------|------|------|------|--------|
| GitHub Pages | ⭐⭐ | 免费 | 中 | ⭐⭐⭐⭐⭐ |
| Vercel | ⭐⭐ | 免费 | 快 | ⭐⭐⭐⭐⭐ |
| Netlify | ⭐ | 免费 | 快 | ⭐⭐⭐⭐ |
| 本地服务器 | ⭐⭐⭐⭐ | 自费 | 快 | ⭐⭐⭐ |
| 阿里云 OSS | ⭐⭐⭐ | 付费 | 快 | ⭐⭐⭐⭐ |

---

## 📊 快速部署（推荐 Vercel）

### 一键部署命令

```bash
# 1. 安装 Vercel（如果未安装）
npm install -g vercel

# 2. 进入目录
cd C:\Users\Administrator\.openclaw\workspace\guojin_auto_trading\backtest

# 3. 部署
vercel --prod
```

### 部署后

- 获得永久在线链接
- 支持分享
- 自动 HTTPS 加密
- 全球 CDN 加速

---

## 🔗 分享链接

部署完成后，你可以：

1. **分享给同事/客户**
   - 发送 URL 链接
   - 手机/电脑均可访问

2. **嵌入 PPT/文档**
   - 插入链接
   - 或截图使用

3. **邮件发送**
   - 直接发送链接
   - 或附件发送 HTML 文件

---

## 📱 移动端适配

H5 页面已完美适配移动端：

- ✅ 响应式布局
- ✅ 触摸友好
- ✅ 加载快速
- ✅ 离线可用（可选 PWA）

---

## 💡 高级功能（可选）

### 添加访问统计

```html
<!-- 在 report.html 的 </body> 前添加 -->
<script async src="https://umami.is/script.js" 
        data-website-id="YOUR_ID">
</script>
```

### 添加分享功能

```html
<!-- 添加分享按钮 -->
<div class="share-buttons">
  <button onclick="share()">分享报告</button>
</div>

<script>
function share() {
  if (navigator.share) {
    navigator.share({
      title: '518880 黄金 ETF 回测报告',
      text: '策略收益 +151.78%，跑赢基准 69.75%',
      url: window.location.href
    });
  }
}
</script>
```

---

## 🆘 需要帮助？

如需协助部署，请告诉我：
1. 你有哪种服务器/平台
2. 是否需要自定义域名
3. 预计访问量大小

我会提供详细的部署指导！

---

*文档创建时间：2026-03-22*
*版本：V1.0.0*
