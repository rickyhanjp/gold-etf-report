# mx-data 集成配置指南

## 📦 概述

mx-data 是东方财富权威金融数据接口，通过 OpenClaw skill 调用，提供：
- 中国/美国 CPI 数据
- 全球/中国/美国 PMI 数据
- 央行利率决策
- 其他宏观经济指标

## 🔧 配置方式

### 方式 1：HTTP API（推荐）

需要 OpenClaw Gateway 开启 HTTP API 支持。

**步骤：**

1. 确认 OpenClaw Gateway 运行中
   ```bash
   openclaw gateway status
   ```

2. 配置环境变量（可选）
   ```bash
   # Windows PowerShell
   $env:OPENCLAW_HOST="http://localhost"
   $env:OPENCLAW_PORT="18789"
   
   # 或编辑 .env 文件
   OPENCLAW_HOST=http://localhost
   OPENCLAW_PORT=18789
   ```

3. 测试连接
   ```python
   from data.mx_data_client import MXDataClient
   
   client = MXDataClient()
   cpi = client.get_cpi("CN", "2023-01", "2024-12")
   print(cpi)
   ```

### 方式 2：会话调用（备用）

如果 HTTP API 不可用，系统会自动降级到会话调用模式：

```
============================================================
mx-data 查询请求
============================================================

请在 OpenClaw 中执行以下查询:
  "中国 CPI 数据 2023-01 至 2024-12"

或者配置 HTTP API 后自动调用。
============================================================
```

此时需要：
1. 在 OpenClaw 聊天中手动执行查询
2. 复制结果到代码中使用

### 方式 3：模拟数据（离线）

当以上两种方式都不可用时，自动使用基于历史统计的模拟数据。

## 📖 使用示例

### 基础用法

```python
from data.mx_data_client import MXDataClient

client = MXDataClient()

# 获取中国 CPI
cpi_cn = client.get_cpi("CN", "2021-01", "2025-12")
print(cpi_cn)

# 获取美国 PMI
pmi_us = client.get_pmi("US", "2021-01", "2025-12")
print(pmi_us)

# 获取完整宏观数据
macro = client.get_all_macro("CN", "2021-01", "2025-12")
print(macro.columns)
```

### 在策略中使用

```python
from data.mx_data_client import MXDataClient
from strategies.gold_trend_factor import GoldTrendFactorStrategy

# 获取宏观数据
client = MXDataClient()
macro = client.get_all_macro("CN", "2023-01", "2024-12")

# 初始化策略
strategy = GoldTrendFactorStrategy()

# 根据宏观数据调整策略参数
for idx, row in macro.iterrows():
    # 高通胀 → 增加黄金仓位
    if row['cpi_yoy'] > 3:
        strategy.position_limit = 0.8
    # 经济收缩 → 避险模式
    elif row['pmi_actual'] < 48:
        strategy.position_limit = 0.6
    else:
        strategy.position_limit = 0.4
    
    print(f"{row['date']}: CPI={row['cpi_yoy']:.1f}%, PMI={row['pmi_actual']:.1f}, "
          f"仓位上限={strategy.position_limit*100:.0f}%")
```

### 与统一 API 集成

```python
from data.macro_api_unified import MacroDataAPI

# 启用 mx-data 优先
api = MacroDataAPI(use_mxdata=True)

# 获取数据（自动优先使用 mx-data）
cpi = api.get_cpi("CN", "2023-01", "2024-12")
pmi = api.get_pmi("GLOBAL", "2023-01", "2024-12")

# 获取完整因子
factors = api.get_all_factors("2023-01", "2024-12")
```

## 🗂️ 缓存管理

数据自动缓存到 `data/cache/mx_data/` 目录，默认 24 小时过期。

```python
# 自定义缓存时间
client = MXDataClient()
client.cache_hours = 48  # 48 小时缓存

# 禁用缓存
client = MXDataClient()
client.use_cache = False

# 手动清除缓存
import os
import shutil

cache_dir = "data/cache/mx_data"
if os.path.exists(cache_dir):
    shutil.rmtree(cache_dir)
    os.makedirs(cache_dir)
    print("缓存已清除")
```

## 📊 数据字段说明

### CPI 数据
| 字段 | 说明 | 单位 |
|------|------|------|
| date | 日期 | - |
| cpi_yoy | 同比增速 | % |
| cpi_mom | 环比增速 | % |

### PMI 数据
| 字段 | 说明 | 单位 |
|------|------|------|
| date | 日期 | - |
| pmi_actual | 实际值 | - |
| pmi_expected | 预期值 | - |

### 完整宏观数据
| 字段 | 说明 | 单位 |
|------|------|------|
| date | 日期 | - |
| cpi_yoy | CPI 同比 | % |
| cpi_mom | CPI 环比 | % |
| pmi_actual | PMI 实际值 | - |
| pmi_expected | PMI 预期值 | - |
| rate | 政策利率 | % |
| real_rate | 实际利率 (rate - cpi) | % |
| economic_surprise | 经济惊喜指数 (pmi_actual - pmi_expected) | - |

## 🔍 故障排查

### 问题 1：mx-data 客户端初始化失败

**症状：**
```
mx-data 客户端初始化失败：No module named 'requests'
```

**解决：**
```bash
pip install requests
```

### 问题 2：HTTP API 连接失败

**症状：**
```
HTTP 调用失败：Connection refused
```

**解决：**
1. 确认 OpenClaw Gateway 运行中
   ```bash
   openclaw gateway status
   ```
2. 检查端口配置
   ```bash
   # 默认端口 18789
   echo $OPENCLAW_PORT
   ```
3. 如无法修复，系统会自动降级到会话调用或模拟数据

### 问题 3：数据格式不正确

**症状：**
```
KeyError: 'cpi_yoy'
```

**解决：**
检查返回数据格式，确保包含必需字段：
```python
result = client.query("中国 CPI 数据 2023 年")
print(result)  # 查看原始返回

# 应该包含 'data' 键，且数据中包含 'cpi_yoy' 或 'value' 字段
```

## 📚 相关文档

- [MACRO_DATA_INTEGRATION.md](MACRO_DATA_INTEGRATION.md) - 宏观数据集成总览
- [DATA_API_GUIDE.md](DATA_API_GUIDE.md) - 数据 API 指南

## 🆘 技术支持

- mx-data skill 文档：`~/.openclaw/workspace/skills/mx-data/SKILL.md`
- OpenClaw 文档：`C:\Users\Administrator\AppData\Roaming\npm\node_modules\openclaw\docs`
- 东方财富数据：https://data.eastmoney.com

---

*文档版本：V1.0.0*
*更新时间：2026-03-23*
