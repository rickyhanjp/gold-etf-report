# 518880 黄金 ETF - 部署检查清单

## ✅ 已完成

- [x] 生成 7 张专业图表
- [x] 创建 H5 报告页面
- [x] 准备部署包
- [x] 启动本地服务器
- [x] 打开部署助手页面

## 🎯 下一步操作（3 选 1）

### 选项 A：Netlify Drop（推荐）⭐⭐⭐

**预计时间：1 分钟**

1. **点击打开 Netlify**
   - 链接：https://app.netlify.com/drop
   - 或等待部署助手页面自动打开

2. **找到部署包文件夹**
   ```
   C:\Users\Administrator\.openclaw\workspace\guojin_auto_trading\deploy_package
   ```

3. **拖拽文件夹到 Netlify**
   - 打开文件资源管理器
   - 定位到上述路径
   - 将整个 `deploy_package` 文件夹拖到 Netlify Drop 页面

4. **等待上传完成**（10-30 秒）

5. **复制在线链接**
   - 格式：`https://xxxx-xxxx.netlify.app/report.html`
   - 保存链接，用于分享

---

### 选项 B：GitHub Pages⭐⭐

**预计时间：5 分钟**

1. **创建 GitHub 仓库**
   - 打开：https://github.com/new
   - 仓库名：`gold-etf-report`
   - 设为 Public

2. **上传文件**
   ```bash
   cd C:\Users\Administrator\.openclaw\workspace\guojin_auto_trading\deploy_package
   git init
   git add .
   git commit -m "518880 backtest report"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/gold-etf-report.git
   git push -u origin main
   ```

3. **启用 Pages**
   - 打开仓库 Settings
   - 点击 Pages
   - Source 选择 `main`
   - 点击 Save

4. **等待部署**（1-2 分钟）

5. **访问链接**
   ```
   https://YOUR_USERNAME.github.io/gold-etf-report/report.html
   ```

---

### 选项 C：Vercel⭐⭐

**预计时间：3 分钟**

1. **安装 Vercel**（如果未安装）
   ```bash
   npm install -g vercel
   ```

2. **部署**
   ```bash
   cd C:\Users\Administrator\.openclaw\workspace\guojin_auto_trading\deploy_package
   vercel --prod
   ```

3. **按提示操作**
   - 登录 Vercel 账号
   - 确认项目设置
   - 等待部署完成

4. **获得链接**
   ```
   https://gold-etf-report.vercel.app/report.html
   ```

---

## 📊 本地预览（备选）

如果在线部署遇到问题，使用本地预览：

**访问地址：** http://localhost:8080/report.html

**分享给同一网络的人：**
```
http://你的IP地址:8080/report.html
```

---

## 🔍 检查清单

部署前确认：

- [ ] 所有图表文件存在
- [ ] report.html 可以正常打开
- [ ] 本地预览正常显示
- [ ] 网络连接正常

部署后检查：

- [ ] 在线链接可以访问
- [ ] 页面显示正常
- [ ] 图表加载正常
- [ ] 手机访问正常

---

## 📱 分享模板

部署完成后，使用以下模板分享：

### 微信/钉钉分享

```
【518880 黄金 ETF 策略回测报告】

📊 策略表现：
• 总收益：+151.78%
• 年化收益：+26.60%
• 超额收益：+69.75%
• 最大回撤：19.58%

📈 查看完整报告：
[你的 Netlify/GitHub 链接]

支持手机/电脑访问
```

### 邮件分享

```
主题：518880 黄金 ETF 策略回测报告 - 超额收益 +69.75%

您好，

附件是 518880 黄金 ETF 量化策略的回测报告。

核心指标：
- 回测期间：2021-2025 年
- 策略收益：+151.78%
- 基准收益：+82.03%
- 超额收益：+69.75%
- 年化收益：+26.60%

在线报告：[你的链接]

策略采用趋势过滤 + 宏观因子打分框架，
成功规避多次市场大跌，值得参考。

如有疑问，欢迎交流。
```

---

## 🆘 故障排查

### 问题 1：Netlify 无法访问
**解决：**
- 检查网络连接
- 使用 Chrome/Edge 浏览器
- 尝试使用 ZIP 文件上传

### 问题 2：页面显示空白
**解决：**
- 等待 1-2 分钟（CDN 缓存）
- 清除浏览器缓存
- 检查文件是否完整上传

### 问题 3：图表不显示
**解决：**
- 确认 PNG 文件已上传
- 检查文件名是否正确
- 重新上传整个文件夹

### 问题 4：本地服务器无法访问
**解决：**
```bash
# 停止当前服务器（Ctrl+C）
# 重新启动
cd deploy_package
python -m http.server 8080
```

---

## 📞 需要进一步协助？

如需帮助，请告诉我：
1. 选择的部署方式
2. 遇到的具体问题
3. 是否需要自定义域名

我会提供详细的解决方案！

---

**当前状态：** ✅ 一切就绪，可以开始部署

**推荐操作：** 打开 https://app.netlify.com/drop 拖拽文件夹

**预计完成时间：** 1 分钟

---

*文档创建时间：2026-03-22 15:43*
