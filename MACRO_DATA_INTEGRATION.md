# 宏观经济数据集成指南

## 📊 数据源对比

| 数据源 | CPI | PMI | 利率 | 优势 | 劣势 |
|--------|-----|-----|------|------|------|
| **mx-data (东方财富)** | ✅ | ✅ | ✅ | 权威、及时、中文 | 需通过 OpenClaw 调用 |
| **akshare** | ✅ | ✅ | ✅ | 免费、数据全 | API 不稳定 |
| **yfinance** | ❌ | ❌ | ⚠️ | 稳定、快速 | 宏观数据少 |
| **FRED (美联储)** | ✅ | ❌ | ✅ | 官方权威 | 仅限美国 |

## 🚀 快速开始

### 1. 使用统一 API

```python
from data.macro_api_unified import MacroDataAPI

api = MacroDataAPI()

# 获取 CPI
cpi_us = api.get_cpi("US", "2021-01", "2025-12")
cpi_cn = api.get_cpi("CN", "2021-01", "2025-12")

# 获取 PMI
pmi_global = api.get_pmi("GLOBAL", "2021-01", "2025-12")
pmi_us = api.get_pmi("US", "2021-01", "2025-12")
pmi_cn = api.get_pmi("CN", "2021-01", "2025-12")

# 获取完整因子数据集
factors = api.get_all_factors("2021-01", "2025-12")
print(factors.columns)
```

### 2. 集成 mx-data（东方财富）

mx-data 是东方财富的权威金融数据接口，通过 OpenClaw skill 调用：

```python
# 在 OpenClaw 会话中调用 mx-data skill
# 示例：查询 CPI 数据

# 方式 1：直接在 OpenClaw 中询问
"查询 2024 年中国 CPI 数据"

# 方式 2：通过 API 调用（需配置 OpenClaw）
import requests

def fetch_mx_cpi(start_date: str, end_date: str):
    """通过 OpenClaw 调用 mx-data 获取 CPI"""
    response = requests.post(
        "http://localhost:18789/api/skill/mx-data",
        json={
            "query": f"中国 CPI 数据 {start_date} 至 {end_date}",
            "data_type": "macro"
        }
    )
    return response.json()
```

### 3. 在策略中使用宏观数据

```python
from data.macro_api_unified import get_macro_factors
from strategies.gold_trend_factor import GoldTrendFactorStrategy

# 获取宏观数据
macro = get_macro_factors("2021-01", "2025-12")

# 初始化策略
strategy = GoldTrendFactorStrategy()

# 遍历宏观数据生成信号
for idx, row in macro.iterrows():
    # CPI 超预期 → 通胀交易
    if row['cpi_us'] > 3.5:
        inflation_signal = 1
    else:
        inflation_signal = -1
    
    # PMI 扩张 → 风险资产偏好
    if row['pmi_global'] > 52:
        risk_on_signal = 1
    elif row['pmi_global'] < 48:
        risk_on_signal = -1
    else:
        risk_on_signal = 0
    
    # 综合信号
    total_signal = inflation_signal + risk_on_signal
    
    print(f"{row['date']}: CPI={row['cpi_us']:.1f}, PMI={row['pmi_global']:.1f}, 信号={total_signal}")
```

## 📈 宏观因子说明

### CPI（消费者物价指数）

| 值范围 | 经济含义 | 黄金影响 |
|--------|----------|----------|
| > 4% | 高通胀 | ✅ 利好（抗通胀需求） |
| 2-4% | 温和通胀 | ➖ 中性 |
| < 2% | 低通胀/通缩 | ❌ 利空（实际利率上升） |

### PMI（采购经理人指数）

| 值范围 | 经济含义 | 黄金影响 |
|--------|----------|----------|
| > 52 | 经济扩张 | ❌ 利空（风险资产偏好） |
| 48-52 | 平稳 | ➖ 中性 |
| < 48 | 经济收缩 | ✅ 利好（避险需求） |

### 利率

| 变化方向 | 经济含义 | 黄金影响 |
|----------|----------|----------|
| 加息周期 | 收紧货币政策 | ❌ 利空（持有成本上升） |
| 降息周期 | 宽松货币政策 | ✅ 利好（持有成本下降） |

## 🔄 数据更新

### 自动缓存

数据自动缓存到 `data/cache/` 目录，默认 24 小时过期：

```python
# 自定义缓存时间
api = MacroDataAPI(cache_hours=48)  # 48 小时缓存

# 禁用缓存
api = MacroDataAPI(use_cache=False)

# 手动清除缓存
import os
cache_dir = "data/cache"
for f in os.listdir(cache_dir):
    os.remove(os.path.join(cache_dir, f))
```

### 定时更新（Cron）

在 Linux/Mac 上设置定时任务：

```bash
# 每天凌晨 2 点更新宏观数据
0 2 * * * cd /path/to/guojin_auto_trading && python data/macro_api_unified.py
```

Windows 任务计划程序：
1. 打开"任务计划程序"
2. 创建基本任务
3. 触发器：每天 02:00
4. 操作：启动程序 `python.exe`
5. 参数：`data/macro_api_unified.py`
6. 起始于：`C:\Users\Administrator\.openclaw\workspace\guojin_auto_trading`

## 🧪 测试与验证

### 单元测试

```python
import unittest
from data.macro_api_unified import MacroDataAPI

class TestMacroData(unittest.TestCase):
    
    def setUp(self):
        self.api = MacroDataAPI(use_cache=False)
    
    def test_cpi_data(self):
        cpi = self.api.get_cpi("US", "2023-01", "2023-12")
        self.assertIsNotNone(cpi)
        self.assertIn('cpi_yoy', cpi.columns)
        self.assertEqual(len(cpi), 12)
    
    def test_pmi_data(self):
        pmi = self.api.get_pmi("GLOBAL", "2023-01", "2023-12")
        self.assertIsNotNone(pmi)
        self.assertIn('pmi_actual', pmi.columns)
    
    def test_all_factors(self):
        factors = self.api.get_all_factors("2023-01", "2023-12")
        self.assertIsNotNone(factors)
        self.assertIn('cpi_us', factors.columns)
        self.assertIn('pmi_global', factors.columns)

if __name__ == '__main__':
    unittest.main()
```

### 数据质量检查

```python
def validate_macro_data(df: pd.DataFrame, expected_columns: list):
    """验证数据质量"""
    issues = []
    
    # 检查必需列
    for col in expected_columns:
        if col not in df.columns:
            issues.append(f"缺少列：{col}")
    
    # 检查缺失值
    missing = df.isnull().sum()
    for col, count in missing.items():
        if count > len(df) * 0.1:  # 超过 10% 缺失
            issues.append(f"{col} 缺失值过多：{count}/{len(df)}")
    
    # 检查异常值
    for col in ['cpi_yoy', 'pmi_actual']:
        if col in df.columns:
            q1 = df[col].quantile(0.01)
            q99 = df[col].quantile(0.99)
            outliers = ((df[col] < q1) | (df[col] > q99)).sum()
            if outliers > 0:
                issues.append(f"{col} 存在 {outliers} 个异常值")
    
    return issues

# 使用示例
factors = get_macro_factors("2021-01", "2025-12")
issues = validate_macro_data(factors, ['cpi_us', 'pmi_global', 'rate_us'])
if issues:
    print("数据质量问题:")
    for issue in issues:
        print(f"  - {issue}")
else:
    print("✓ 数据质量检查通过")
```

## 📚 相关文档

- [DATA_API_GUIDE.md](DATA_API_GUIDE.md) - 原始数据 API 指南
- [CPI_PMI_BACKTEST_ANALYSIS.md](../backtest/CPI_PMI_BACKTEST_ANALYSIS.md) - CPI+PMI 回测分析
- [REAL_DATA_BACKTEST_REPORT.md](../backtest/REAL_DATA_BACKTEST_REPORT.md) - 真实数据回测报告

## 🆘 常见问题

### Q: mx-data 如何调用？
A: mx-data 是 OpenClaw skill，通过自然语言调用。在 OpenClaw 会话中直接询问即可。

### Q: 数据获取失败怎么办？
A: 系统会自动降级到模拟数据。检查网络连接或安装 akshare/yfinance。

### Q: 如何接入自己的数据源？
A: 继承 `MacroDataAPI` 类，重写 `get_cpi`/`get_pmi` 方法即可。

### Q: 数据频率是多少？
A: 宏观数据通常是月度发布，部分国家 PMI 有月度数据。

---

*文档版本：V1.0.0*
*更新时间：2026-03-23*
