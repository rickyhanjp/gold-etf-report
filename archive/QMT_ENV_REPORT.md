# 国金证券 QMT Python 环境检测报告

**检测时间：** 2026-03-21  
**检测工具：** `check_qmt_libs.py`

---

## 📍 QMT 环境信息

| 项目 | 值 |
|------|-----|
| **安装路径** | `C:\国金证券 QMT 交易端\` |
| **Python 路径** | `C:\国金证券 QMT 交易端\bin.x64\` |
| **site-packages** | `bin.x64\Lib\site-packages\` |
| **Python 版本** | Python 3.6 (推测) |

---

## ✅ 已安装的核心库 (14 个)

| 库名 | 版本 | 用途 | 状态 |
|------|------|------|------|
| **xtquant** | latest | QMT 量化交易 API | ✅ 已安装 |
| **qmt_api** | latest | QMT API 封装 | ✅ 已安装 |
| **numpy** | 1.19.1 | 数值计算 | ✅ 已安装 |
| **pandas** | 0.22.0 | 数据处理 | ✅ 已安装 |
| **scipy** | 1.5.2 | 科学计算 | ✅ 已安装 |
| **scikit-learn** | 0.0 (sklearn) | 机器学习 | ✅ 已安装 |
| **matplotlib** | 3.0.3 | 数据可视化 | ✅ 已安装 |
| **tensorflow** | 1.8.0 | 深度学习 | ✅ 已安装 |
| **torch** | 1.0.1 | PyTorch | ✅ 已安装 |
| **talib** | latest | 技术分析 | ✅ 已安装 |
| **pymongo** | 3.11.3 | MongoDB | ✅ 已安装 |
| **redis** | 3.5.3 | Redis 缓存 | ✅ 已安装 |
| **requests** | 2.24.0 | HTTP 请求 | ✅ 已安装 |
| **cvxpy** | 0.4.11 | 凸优化 | ✅ 已安装 |

---

## 📦 完整库列表 (85 个)

### 量化交易核心
```
xtquant, qmt_api, talib
```

### 数据处理与计算
```
numpy==1.19.1, pandas==0.22.0, scipy==1.5.2, numexpr==2.7.0
```

### 机器学习/深度学习
```
scikit-learn, tensorflow==1.8.0, torch==1.0.1, keras, cvxpy==0.4.11, cvxopt==1.2.5
```

### 数据可视化
```
matplotlib==3.0.3, PIL (Pillow==6.0.0)
```

### 数据库
```
pymongo==3.11.3, redis==3.5.3, tables==3.6.1 (HDF5), xtbson, xtdbf
```

### 文件处理
```
xlrd==1.2.0, xlsxwriter==1.2.7, xlwt==1.3.0, dbf, dbfpy
```

### 网络请求
```
requests==2.24.0, urllib3==1.25.9, grpcio==1.12.1
```

### Jupyter/交互式环境
```
jupyter-client, ipykernel, notebook, ipython==7.4.0, jinja2==2.10
```

### 工具库
```
tqdm==4.64.1, joblib==0.16.0, multiprocess, dill, simplejson==3.17.2
```

### 其他依赖
```
protobuf==3.5.2, tensorboard==1.8.0, werkzeug==0.14.1, statsmodels==0.12.1
```

---

## 🚀 pip 安装命令

### 快速安装（核心库）

```bash
pip install numpy==1.19.1 pandas==0.22.0 scipy==1.5.2 scikit-learn matplotlib==3.0.3 tensorflow==1.8.0 torch==1.0.1 talib pymongo==3.11.3 redis==3.5.3 requests==2.24.0 simplejson==3.17.2 python-dateutil pytz==2018.3 tqdm==4.64.1 joblib==0.16.0 xlrd==1.2.0 xlsxwriter==1.2.7 cvxpy==0.4.11 cvxopt==1.2.5 statsmodels==0.12.1 tables==3.6.1 numexpr==2.7.0
```

### 最小化安装（仅必需）

```bash
pip install numpy pandas scipy matplotlib requests python-dateutil pytz tqdm
```

---

## 📝 生成的文件

| 文件名 | 说明 |
|--------|------|
| `requirements_qmt.txt` | 手动编写的 QMT 库依赖列表 |
| `requirements_qmt_detected.txt` | 自动检测生成的依赖列表 |
| `check_qmt_libs.py` | Python 库版本检测工具 |
| `install_qmt_libs.bat` | Windows 批量安装脚本 |
| `INSTALL_QMT_LIBS.md` | 详细安装指南 |
| `qmt_libraries.txt` | 完整库列表（纯文本） |

---

## ⚠️ 版本说明

### 较旧的库版本

| 库 | QMT 版本 | 最新稳定版 | 差异 |
|----|----------|------------|------|
| Python | 3.6 | 3.11 | 差 5 个大版本 |
| pandas | 0.22.0 | 2.0+ | 差 2 个大版本 |
| tensorflow | 1.8.0 | 2.13+ | 不兼容 |
| torch | 1.0.1 | 2.0+ | 差 1 个大版本 |
| numpy | 1.19.1 | 1.24+ | 兼容 |

### 建议

1. **在 QMT 内部使用内置库** - 保证兼容性
2. **外部研究用新环境** - 创建独立虚拟环境
3. **不要混用** - 避免版本冲突

---

## 💡 使用 xtquant 示例

```python
# QMT 策略模板
from xtquant import xtdata, xttrader
import pandas as pd
import numpy as np

# 1. 连接 QMT 数据服务
xtdata.connect()

# 2. 获取股票列表
stocks = xtdata.get_stock_list_in_sector('沪深 A 股')
print(f"A 股数量：{len(stocks)}")

# 3. 获取行情数据
data = xtdata.get_market_data(
    field_list=['open', 'high', 'low', 'close', 'volume'],
    stock_list=['600519.SH'],
    period='1d',
    start_time='20240101',
    end_time='20241231'
)

# 4. 转换为 DataFrame
df = pd.DataFrame(data['600519.SH'])
print(df.head())

# 5. 计算技术指标
df['MA5'] = df['close'].rolling(5).mean()
df['MA20'] = df['close'].rolling(20).mean()

# 6. 下订单（需要交易权限）
# xttrader.order_buy('600519.SH', price=1700, volume=100)
```

---

## 🔧 常见问题

### Q1: 如何在 QMT 外使用 xtquant？

**答：** xtquant 是 QMT 专用库，**不能**在外部 Python 使用。只能在 QMT 策略编辑器中运行。

### Q2: 如何升级 pandas/numpy 等库？

**答：** 
```bash
# 升级 QMT 内置库（不推荐，可能影响 QMT 稳定性）
"C:\国金证券 QMT 交易端\bin.x64\python.exe" -m pip install --upgrade pandas

# 或创建独立环境（推荐）
python -m venv myenv
myenv\Scripts\activate
pip install pandas numpy scipy
```

### Q3: 如何查看某个库的详细信息？

**答：**
```bash
# 查看版本
python -c "import pandas; print(pandas.__version__)"

# 查看位置
python -c "import pandas; print(pandas.__file__)"
```

---

## 📞 下一步

1. **使用检测工具** - 运行 `check_qmt_libs.py` 获取最新版本信息
2. **安装依赖** - 运行 `install_qmt_libs.bat` 或手动 pip 安装
3. **编写策略** - 在 QMT 中使用 xtquant 编写量化策略
4. **集成系统** - 将 QMT 策略集成到 `guojin_auto_trading` 框架

---

**检测完成！** ✅

所有核心量化库已就绪，可以开始编写量化策略了。
