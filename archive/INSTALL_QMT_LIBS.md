# 国金证券 QMT Python 库安装指南

## 📦 已检测到的 QMT 环境

**QMT 安装路径：** `C:\国金证券 QMT 交易端\`  
**Python 库路径：** `C:\国金证券 QMT 交易端\bin.x64\Lib\site-packages`  
**Python 版本：** Python 3.6 (QMT 内置)

---

## ✅ 已安装的主要量化库

| 库名 | 用途 | 状态 |
|------|------|------|
| **xtquant** | QMT 量化交易 API | ✓ 已安装 |
| **qmt_api** | QMT API 封装 | ✓ 已安装 |
| **numpy** | 数值计算 | ✓ 已安装 |
| **pandas** | 数据处理 | ✓ 已安装 |
| **scipy** | 科学计算 | ✓ 已安装 |
| **scikit-learn** | 机器学习 | ✓ 已安装 |
| **matplotlib** | 数据可视化 | ✓ 已安装 |
| **tensorflow** | 深度学习 | ✓ 已安装 (v1.8.0) |
| **torch** | 深度学习 | ✓ 已安装 (v1.0.1) |
| **talib** | 技术分析 | ✓ 已安装 |
| **pymongo** | MongoDB | ✓ 已安装 |
| **redis** | Redis 缓存 | ✓ 已安装 |

---

## 🚀 安装方法

### 方法 1：使用安装脚本（推荐）

双击运行：
```
install_qmt_libs.bat
```

### 方法 2：手动 pip 安装

**核心量化库：**
```bash
pip install numpy pandas scipy scikit-learn matplotlib talib xtquant qmt_api pymongo redis requests simplejson python-dateutil pytz tqdm joblib xlrd xlsxwriter
```

**完整库列表：**
```bash
pip install numpy==1.19.1 pandas==0.22.0 scipy==1.5.2 scikit-learn==0.23.2 matplotlib==3.0.3 tensorflow==1.8.0 torch==1.0.1 talib pymongo==3.11.3 redis==3.5.3 requests==2.24.0 simplejson==3.17.2 python-dateutil pytz==2018.3 tqdm==4.64.1 joblib==0.16.0 xlrd==1.2.0 xlsxwriter==1.2.7 cvxpy cvxopt==1.2.5 statsmodels==0.12.1 tables==3.6.1 numexpr==2.7.0
```

### 方法 3：使用 QMT 内置 Python

QMT 自带 Python 环境，可以直接在 QMT 策略中使用：

```python
# QMT 策略示例
from xtquant import xtdata
import pandas as pd
import numpy as np

# 获取行情数据
data = xtdata.get_market_data(field_list=['open', 'high', 'low', 'close'], 
                               stock_list=['600519.SH'], 
                               period='1d',
                               start_time='20240101',
                               end_time='20241231')
print(data)
```

---

## 📝 完整库列表 (85 个)

```
absl, aenum, astor, attr, backcall, bleach, bson, caffe2, certifi, 
chardet, colorama, cvxopt, cvxpy, dateutil, dbf, dbfpy, defusedxml, 
dill, fastcache, gast, google, gridfs, grpc, html5lib, idna, 
importlib_resources, ipykernel, IPython, ipython_genutils, jedi, 
jinja2, joblib, jsonschema, jupyter_client, jupyter_core, markdown, 
markupsafe, matplotlib, multiprocess, nbconvert, nbformat, notebook, 
numexpr, numpy, pandas, parso, patsy, PIL, pip, pkg_resources, 
prometheus_client, prompt_toolkit, pygments, pymongo, pyreadline, 
pyrsistent, pytz, qmt_api, redis, requests, scipy, scs, send2trash, 
setuptools, simplejson, sklearn, statsmodels, tables, talib, 
tensorboard, tensorflow, terminado, testpath, tlz, toolz, torch, 
tornado, tqdm, traitlets, urllib3, wcwidth, webencodings, werkzeug, 
wheel, xlrd, xlsxwriter, xlwt, xtbson, xtdbf, xtquant, zmq
```

完整列表已保存到：`qmt_libraries.txt`

---

## 🔧 常见问题

### 1. pip 命令不可用

**解决方案：**
```bash
# 使用 QMT 内置 pip
"C:\国金证券 QMT 交易端\bin.x64\python.exe" -m pip install <包名>

# 或使用完整路径
"C:\国金证券 QMT 交易端\bin.x64\Scripts\pip.exe" install <包名>
```

### 2. 找不到 xtquant 模块

**原因：** xtquant 是 QMT 专用库，只能在 QMT 内置 Python 中使用

**解决方案：**
- 在 QMT 策略编辑器中编写代码
- 使用 QMT 内置 Python 运行脚本
- 不要尝试在外部 Python 环境使用 xtquant

### 3. 版本冲突

QMT 内置的库版本较旧（如 TensorFlow 1.8.0），如需新版本：

```bash
# 升级特定库
pip install --upgrade numpy pandas

# 或创建独立虚拟环境
python -m venv venv
venv\Scripts\activate
pip install numpy pandas scipy scikit-learn
```

---

## 💡 使用建议

### 在 QMT 中使用

```python
# QMT 策略模板
from xtquant import xtdata, xttrader
import pandas as pd
import numpy as np

def init(context):
    """初始化"""
    context.stock = '600519.SH'
    context.grid_spacing = 0.02
    
def handle_data(context, data):
    """行情回调"""
    # 获取实时行情
    quote = xtdata.get_quote(context.stock)
    current_price = quote['lastPrice']
    
    # 执行交易逻辑
    # ...
```

### 在外部 Python 中使用

```python
# 外部研究环境（不能使用 xtquant）
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# 从 QMT 导出数据后分析
df = pd.read_csv('qmt_export.csv')
df.plot()
plt.show()
```

---

## 📊 库分类说明

### 核心量化库
- **xtquant** - QMT 量化接口（核心）
- **qmt_api** - QMT API 封装
- **talib** - 技术指标计算

### 数据处理
- **numpy** - 数值计算
- **pandas** - 数据分析
- **scipy** - 科学计算

### 机器学习
- **scikit-learn** - 传统 ML
- **tensorflow** - 深度学习
- **torch** - PyTorch

### 数据获取
- **requests** - HTTP 请求
- **pymongo** - MongoDB
- **redis** - 缓存

### 可视化
- **matplotlib** - 绘图
- **jupyter** - 交互式环境

### 文件处理
- **xlrd/xlsxwriter** - Excel
- **tables** - HDF5

---

## ⚠️ 注意事项

1. **QMT 内置 Python 3.6** - 部分新库可能不兼容
2. **xtquant 仅限 QMT 内部** - 外部 Python 无法使用
3. **版本较旧** - TensorFlow 1.8.0 等库版本较老
4. **建议分离环境** - 研究用新环境，交易用 QMT 环境

---

## 📞 需要帮助？

如果需要：
- 配置 QMT API 连接
- 编写量化策略
- 集成到自动交易系统
- 升级 Python 环境

请随时告诉我！

---

**文档生成时间：** 2026-03-21  
**QMT 版本：** 国金证券 QMT 交易端  
**Python 版本：** 3.6
