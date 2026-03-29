# 国金证券 QMT Python 环境安装报告

**安装时间：** 2026-03-21  
**Python 版本：** 3.14.3  
**安装状态：** ✅ 成功

---

## ✅ 安装成功的库 (19 个核心库 + 30 个依赖)

### 核心量化库

| 库名 | 版本 | 用途 | 状态 |
|------|------|------|------|
| **numpy** | 2.4.3 | 数值计算 | ✅ |
| **pandas** | 3.0.1 | 数据处理 | ✅ |
| **scipy** | 1.17.1 | 科学计算 | ✅ |
| **scikit-learn** | 1.8.0 | 机器学习 | ✅ |
| **matplotlib** | 3.10.8 | 数据可视化 | ✅ |
| **cvxpy** | 1.8.1 | 凸优化 | ✅ |
| **statsmodels** | 0.14.6 | 统计建模 | ✅ |
| **tables** | 3.11.1 | HDF5 支持 | ✅ |

### 数据库与缓存

| 库名 | 版本 | 用途 | 状态 |
|------|------|------|------|
| **pymongo** | 4.16.0 | MongoDB | ✅ |
| **redis** | 7.3.0 | Redis 缓存 | ✅ |

### 网络与工具

| 库名 | 版本 | 用途 | 状态 |
|------|------|------|------|
| **requests** | 2.32.5 | HTTP 请求 | ✅ |
| **simplejson** | 3.20.2 | JSON 处理 | ✅ |
| **python-dateutil** | 2.9.0 | 日期处理 | ✅ |
| **pytz** | 2026.1 | 时区 | ✅ |
| **tqdm** | 4.67.3 | 进度条 | ✅ |
| **joblib** | 1.5.3 | 并行计算 | ✅ |

### 文件处理

| 库名 | 版本 | 用途 | 状态 |
|------|------|------|------|
| **xlrd** | 2.0.2 | 读 Excel | ✅ |
| **xlsxwriter** | 3.2.9 | 写 Excel | ✅ |

---

## ⚠️ 未安装的库

| 库名 | 原因 | 替代方案 |
|------|------|----------|
| **talib** | Python 3.14 无预编译包 | 使用 TA-Lib C 库或手动编译 |
| **xtquant** | QMT 专用库 | 仅在 QMT 内部使用 |
| **qmt_api** | QMT 专用库 | 仅在 QMT 内部使用 |
| **tensorflow** | Python 3.14 不兼容 | 使用外部 Python 3.10 环境 |
| **torch** | Python 3.14 不兼容 | 使用外部 Python 3.10 环境 |

---

## 📦 安装的依赖库 (30 个)

```
certifi, cffi, charset_normalizer, clarabel, colorama, contourpy, 
cycler, dnspython, fonttools, highspy, idna, jinja2, kiwisolver, 
MarkupSafe, msgpack, ndindex, numexpr, osqp, packaging, patsy, 
pillow, py-cpuinfo, pycparser, pyparsing, scs, setuptools, six, 
threadpoolctl, tzdata, urllib3
```

---

## 🔧 安装命令

```bash
pip install numpy pandas scipy scikit-learn matplotlib pymongo redis requests simplejson python-dateutil pytz tqdm joblib xlrd xlsxwriter cvxpy statsmodels tables numexpr
```

---

## 🧪 验证测试

```python
# 运行以下代码验证安装
import numpy as np
import pandas as pd
from scipy import stats
from sklearn.linear_model import LinearRegression
import matplotlib.pyplot as plt

# 测试 numpy
arr = np.array([1, 2, 3, 4, 5])
print(f"numpy 测试：{arr.mean()}")

# 测试 pandas
df = pd.DataFrame({'A': [1, 2, 3], 'B': [4, 5, 6]})
print(f"pandas 测试:\n{df.mean()}")

# 测试 scipy
data = np.random.randn(100)
print(f"scipy 测试：均值={np.mean(data):.3f}, 标准差={np.std(data):.3f}")

# 测试 sklearn
X = np.random.randn(100, 2)
y = X[:, 0] + X[:, 1] + np.random.randn(100) * 0.1
model = LinearRegression().fit(X, y)
print(f"sklearn 测试：R²={model.score(X, y):.3f}")

print("\n[OK] 所有库验证通过！")
```

---

## 📊 版本对比

| 库 | QMT 内置版本 | 新安装版本 | 差异 |
|----|-------------|-----------|------|
| numpy | 1.19.1 | 2.4.3 | ✅ 新 2 个大版本 |
| pandas | 0.22.0 | 3.0.1 | ✅ 新 2 个大版本 |
| scipy | 1.5.2 | 1.17.1 | ✅ 新 1 个大版本 |
| scikit-learn | 0.0 | 1.8.0 | ✅ 全新版本 |
| matplotlib | 3.0.3 | 3.10.8 | ✅ 新 1 个大版本 |

---

## 💡 使用建议

### 1. 环境分离

**QMT 内置环境** (Python 3.6)
- 用于：实盘交易、QMT 策略
- 位置：`C:\国金证券 QMT 交易端\bin.x64\`
- 特点：包含 xtquant、qmt_api

**新安装环境** (Python 3.14)
- 用于：策略研究、回测、数据分析
- 位置：系统 Python
- 特点：库版本新、功能全

### 2. 工作流建议

```
1. 在新环境中开发和回测策略
   ↓
2. 验证策略有效性
   ↓
3. 移植到 QMT 环境（使用 xtquant）
   ↓
4. 实盘运行
```

### 3. 代码示例

**新环境（研究用）：**
```python
import pandas as pd
import numpy as np
from sklearn.ensemble import RandomForestClassifier

# 数据分析和模型训练
df = pd.read_csv('stock_data.csv')
# ... 策略研究
```

**QMT 环境（交易用）：**
```python
from xtquant import xtdata, xttrader
import pandas as pd

# 实盘交易
xtdata.connect()
data = xtdata.get_market_data('600519.SH')
# ... 实盘下单
```

---

## ⚙️ 下一步

### 立即可用

1. ✅ 数据分析 - pandas, numpy, scipy
2. ✅ 机器学习 - scikit-learn
3. ✅ 数据可视化 - matplotlib
4. ✅ 优化问题 - cvxpy
5. ✅ 统计建模 - statsmodels

### 需要额外配置

1. ⚠️ TA-Lib - 需要手动编译或使用 Docker
2. ⚠️ TensorFlow/PyTorch - 需要 Python 3.10 环境
3. ⚠️ xtquant - 只能在 QMT 内部使用

---

## 📝 生成的文件

| 文件 | 说明 |
|------|------|
| `INSTALL_REPORT.md` | 本安装报告 |
| `requirements_installed.txt` | 已安装库列表 |
| `test_installation.py` | 安装验证脚本 |

---

## ✅ 安装总结

**成功安装 19 个核心库 + 30 个依赖库**

所有量化交易所需的基础库已就绪，可以开始：
- ✅ 数据分析与处理
- ✅ 策略研究与回测
- ✅ 机器学习模型训练
- ✅ 统计分析与建模
- ✅ 数据可视化

**注意：** xtquant 和 qmt_api 需在 QMT 环境中使用。

---

**安装完成！** 🎉
