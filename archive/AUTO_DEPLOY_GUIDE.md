# 🚀 自动部署指南

## 自动发布回测报告到 GitHub Pages

---

## 📋 功能说明

每次运行回测后，自动：
1. ✅ 生成 HTML5 交互式回测报告
2. ✅ 推送到 GitHub 仓库
3. ✅ 触发 GitHub Pages 部署
4. ✅ 覆盖 `report.html`

---

## 🔧 配置步骤

### 步骤 1: 获取 GitHub Token

1. 访问：https://github.com/settings/tokens
2. 点击 **Generate new token (classic)**
3. 填写说明：`Auto Deploy Bot`
4. 勾选权限：
   - ✅ **repo** (Full control of private repositories)
   - ✅ **workflow** (Update GitHub Action workflows)
5. 点击 **Generate token**
6. **复制 Token**（只显示一次，保存好！）

---

### 步骤 2: 配置 Token

#### 方式 A: 使用 .env 文件（推荐）

1. 复制示例文件：
   ```bash
   cd C:\Users\Administrator\.openclaw\workspace\guojin_auto_trading
   copy .env.example .env
   ```

2. 编辑 `.env` 文件：
   ```bash
   notepad .env
   ```

3. 填入你的 Token：
   ```
   GITHUB_TOKEN=ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
   ```

#### 方式 B: 命令行参数

每次运行时传入 Token：
```bash
python auto_deploy_to_github.py --token ghp_xxxxx...
```

#### 方式 C: 环境变量

```bash
# Windows PowerShell
$env:GITHUB_TOKEN="ghp_xxxxx..."

# Linux/Mac
export GITHUB_TOKEN="ghp_xxxxx..."
```

---

## 🚀 使用方法

### 方式 1: 一键部署（推荐）

```bash
cd C:\Users\Administrator\.openclaw\workspace\guojin_auto_trading
python auto_deploy_to_github.py
```

**完整流程**:
1. 运行回测
2. 生成 HTML5 报告
3. 推送到 GitHub
4. 触发 Pages 部署

---

### 方式 2: 仅部署（不重新回测）

```bash
python auto_deploy_to_github.py --skip-backtest
```

**适用场景**:
- 已运行过回测
- 只需更新报告页面

---

### 方式 3: 自定义参数

```bash
python auto_deploy_to_github.py \
  --owner rickyhanjp \
  --repo gold-etf-report \
  --token ghp_xxxxx... \
  --branch main
```

---

## 📝 集成到回测脚本

### 修改回测脚本

在回测脚本末尾添加：

```python
# 回测完成后自动部署
if __name__ == "__main__":
    # ... 回测代码 ...
    
    # 自动部署到 GitHub Pages
    import subprocess
    subprocess.run([
        sys.executable,
        "auto_deploy_to_github.py",
        "--skip-backtest"  # 已运行过回测
    ])
```

---

### 创建批处理脚本（Windows）

创建 `run_backtest_and_deploy.bat`:

```batch
@echo off
echo ========================================
echo 518880 回测 + 自动部署
echo ========================================

cd /d "%~dp0"

echo.
echo [1/2] 运行回测...
python backtest\backtest_github_2023_2026.py

echo.
echo [2/2] 部署到 GitHub Pages...
python auto_deploy_to_github.py --skip-backtest

echo.
echo ========================================
echo 完成！
echo ========================================
echo.
echo 访问：https://rickyhanjp.github.io/gold-etf-report/report.html

pause
```

**使用方法**:
```bash
run_backtest_and_deploy.bat
```

---

### 创建 Shell 脚本（Linux/Mac）

创建 `run_backtest_and_deploy.sh`:

```bash
#!/bin/bash

echo "========================================"
echo "518880 回测 + 自动部署"
echo "========================================"

cd "$(dirname "$0")"

echo ""
echo "[1/2] 运行回测..."
python backtest/backtest_github_2023_2026.py

echo ""
echo "[2/2] 部署到 GitHub Pages..."
python auto_deploy_to_github.py --skip-backtest

echo ""
echo "========================================"
echo "完成！"
echo "========================================"
echo ""
echo "访问：https://rickyhanjp.github.io/gold-etf-report/report.html"
```

**使用方法**:
```bash
chmod +x run_backtest_and_deploy.sh
./run_backtest_and_deploy.sh
```

---

## ⏰ 定时自动部署

### Windows 任务计划程序

1. 打开 **任务计划程序**
2. 创建基本任务
3. 名称：`518880 自动回测部署`
4. 触发器：每天/每周
5. 操作：启动程序
   - 程序：`C:\Python314\python.exe`
   - 参数：`auto_deploy_to_github.py`
   - 起始于：`C:\Users\Administrator\.openclaw\workspace\guojin_auto_trading`

---

### Linux Cron

编辑 crontab：
```bash
crontab -e
```

添加（每天凌晨 2 点）：
```
0 2 * * * cd /path/to/guojin_auto_trading && /usr/bin/python3 auto_deploy_to_github.py
```

---

## 📊 输出示例

```
======================================================================
🚀 GitHub Pages 自动部署
======================================================================
仓库：rickyhanjp/gold-etf-report
分支：main
目标：https://rickyhanjp.github.io/gold-etf-report/report.html
时间：2026-03-24 00:45:00

======================================================================
检查前置条件
======================================================================
✓ Git 已安装：git version 2.40.0.windows.1
✓ GitHub Token 已配置
✓ 报告文件存在：backtest/report.html
  文件大小：21.2 KB

======================================================================
准备仓库
======================================================================
克隆仓库：https://github.com/rickyhanjp/gold-etf-report
✓ 仓库已克隆

======================================================================
复制报告文件
======================================================================
✓ 报告已复制：report.html
  目标位置：.github_deploy_temp/gold-etf-report/report.html

======================================================================
提交并推送
======================================================================
✓ 已提交：Auto-update: 回测报告 2026-03-24 00:45
推送到 GitHub...
✓ 推送成功

======================================================================
检查部署状态
======================================================================
Pages 地址：https://rickyhanjp.github.io/gold-etf-report/report.html
等待部署...

✓ 部署已触发
查看进度：https://github.com/rickyhanjp/gold-etf-report/actions
访问页面：https://rickyhanjp.github.io/gold-etf-report/report.html

清理临时文件...
✓ 已清理：.github_deploy_temp

======================================================================
✅ 部署完成！
======================================================================

访问链接：https://rickyhanjp.github.io/gold-etf-report/report.html
强制刷新：https://rickyhanjp.github.io/gold-etf-report/report.html?t=1711248300
查看部署：https://github.com/rickyhanjp/gold-etf-report/actions
```

---

## ⚠️ 常见问题

### Q: Token 无效？
**A**: 检查：
1. Token 是否正确复制（无空格）
2. Token 权限是否包含 `repo`
3. Token 是否过期

### Q: 推送失败？
**A**: 检查：
1. 仓库名是否正确
2. 分支名是否正确（main vs master）
3. 是否有写入权限

### Q: Pages 不更新？
**A**: 等待 2-5 分钟，GitHub 需要时间构建

### Q: 如何跳过回测？
**A**: 使用 `--skip-backtest` 参数

---

## 🔗 相关脚本

| 脚本 | 说明 |
|------|------|
| [`auto_deploy_to_github.py`](auto_deploy_to_github.py) | **自动部署主脚本** ⭐ |
| [`backtest/backtest_github_2023_2026.py`](backtest/backtest_github_2023_2026.py) | 回测脚本 |
| [`backtest/generate_h5_report.py`](backtest/generate_h5_report.py) | 生成 HTML 报告 |

---

## 📞 快速命令

### 首次配置
```bash
# 1. 获取 Token
https://github.com/settings/tokens

# 2. 配置 .env
copy .env.example .env
notepad .env

# 3. 测试部署
python auto_deploy_to_github.py
```

### 日常使用
```bash
# 完整流程（回测 + 部署）
python auto_deploy_to_github.py

# 仅部署（已有回测数据）
python auto_deploy_to_github.py --skip-backtest

# 查看帮助
python auto_deploy_to_github.py --help
```

---

**配置完成后，每次只需运行**:
```bash
python auto_deploy_to_github.py
```

**自动完成所有步骤！** 🚀

---

**更新时间**: 2026-03-24 00:46  
**用户名**: rickyhanjp  
**仓库**: gold-etf-report
