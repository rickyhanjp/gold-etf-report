# 国金证券量化交易系统 - 快速开始

## 📋 前置条件

### 1. 开通国金证券量化交易权限

```
✅ 账户资产 ≥ 50 万元
✅ 完成风险测评（C4 及以上）
✅ 签署《量化交易风险揭示书》
✅ 下载并安装 QMT/Ptrade 终端
✅ 获取 API 接入凭证
```

**联系谁开通？**
- 拨打国金证券客服：95310
- 联系您的客户经理
- 前往开户营业部办理

### 2. 获取 API 文档和 SDK

开通权限后，向技术支持索取：
- QMT/Ptrade API 官方文档
- xtquant SDK 安装包
- API 接入示例代码
- 技术支持群二维码

---

## 🚀 安装步骤

### 步骤 1：克隆/下载项目

```bash
# 如果已有项目
cd C:\Users\Administrator\.openclaw\workspace\guojin_auto_trading

# 或从 Git 下载
git clone <your-repo-url>
cd guojin_auto_trading
```

### 步骤 2：安装 Python 依赖

```bash
# 创建虚拟环境（推荐）
python -m venv venv
venv\Scripts\activate  # Windows
# source venv/bin/activate  # Linux/Mac

# 安装依赖
pip install -r requirements.txt
```

### 步骤 3：安装 QMT SDK

```bash
# 方法 1：从国金证券获取的离线包安装
pip install xtquant-xxx.whl

# 方法 2：如果官方提供 pip 源
pip install xtquant -i https://pypi.gjzq.com.cn/simple
```

### 步骤 4：配置账户信息

编辑 `config/settings.yaml`：

```yaml
account:
  user_id: "YOUR_ACCOUNT_ID"  # 你的资金账号
  api_password: "YOUR_PASSWORD"  # API 密码（如有）
  
api:
  type: "qmt"  # 或 ptrade
  endpoint: "127.0.0.1:8888"  # API 地址
```

编辑 `config/strategies.yaml` 配置策略参数。

### 步骤 5：运行系统

```bash
# 命令行模式
python main.py

# UI 界面模式
python main.py --ui
```

---

## 📁 项目结构

```
guojin_auto_trading/
├── README.md              # 项目说明
├── API_GUIDE.md          # API 使用指南 ⭐
├── QUICKSTART.md         # 本文件
├── main.py               # 主程序入口
├── requirements.txt      # 依赖包
│
├── config/
│   ├── settings.yaml     # 主配置
│   ├── strategies.yaml   # 策略配置
│   └── risk_params.yaml  # 风控参数
│
├── core/
│   └── api_client.py     # API 客户端封装
│
├── strategies/
│   ├── stock_selector.py     # 选股模块
│   ├── grid_trading.py       # 网格交易
│   └── momentum_strategy.py  # 动量策略
│
├── risk_control/
│   ├── position_limit.py     # 仓位控制
│   └── stop_loss.py          # 止损止盈
│
├── backtest/
│   ├── engine.py             # 回测引擎
│   └── analyzer.py           # 绩效分析
│
├── ui/
│   ├── dashboard.py          # 主界面
│   └── config_panel.py       # 配置面板
│
└── logs/                     # 日志目录（自动创建）
```

---

## ⚙️ 配置说明

### 主配置 (settings.yaml)

```yaml
account:
  user_id: "12345678"           # 资金账号
  api_password: "password"      # API 密码

api:
  type: "qmt"                   # API 类型
  endpoint: "127.0.0.1:8888"    # 连接地址
  heartbeat_interval: 30        # 心跳间隔（秒）

trading:
  market: "A"                   # A 股市场
  check_trading_hours: true     # 检查交易时间

risk:
  enabled: true                 # 启用风控
  max_position_per_stock: 20    # 单只股票最大仓位%
  max_total_position: 80        # 总仓位上限%
  stop_loss_percent: 5          # 止损比例%
  take_profit_percent: 10       # 止盈比例%
```

### 策略配置 (strategies.yaml)

```yaml
strategies:
  grid_trading:
    enabled: true
    symbols:
      - "600519.SH"
      - "000858.SZ"
    grid_spacing: 2.0           # 网格间距%
    grid_amount: 10000          # 每格金额（元）
    grid_layers: 10             # 网格层数

  momentum:
    enabled: false
    breakout_days: 20           # 突破 N 日高点
    volume_multiplier: 2.0      # 成交量放大倍数
```

### 风控配置 (risk_params.yaml)

```yaml
position_control:
  max_position_per_stock: 20    # 单只股票最大%
  max_total_position: 80        # 总仓位最大%

stop_loss:
  enabled: true
  fixed_percent: 5              # 固定止损%
  trailing_percent: 3           # 追踪止损%

daily_limit:
  max_daily_buy: 500000         # 单日最大买入（元）
  max_order_amount: 100000      # 单笔最大金额（元）
```

---

## 🎯 使用示例

### 1. 运行网格交易

```bash
# 确保配置中启用了网格策略
# 编辑 config/strategies.yaml:
# strategies.grid_trading.enabled: true

python main.py
```

### 2. 运行回测

```python
from backtest.engine import BacktestEngine, BacktestConfig
from strategies.momentum_strategy import MomentumStrategy

# 配置回测
config = BacktestConfig(
    start_date='2024-01-01',
    end_date='2024-12-31',
    initial_cash=1000000,
    commission_rate=0.0003,
    stamp_tax_rate=0.001,
    slippage=0.002
)

# 创建引擎
engine = BacktestEngine(config)

# 加载数据
engine.load_data('600519.SH', klines)

# 运行回测
strategy = MomentumStrategy({...}, None)
result = engine.run(strategy)

# 输出结果
print(f"总收益：{result.total_return*100:.2f}%")
print(f"最大回撤：{result.max_drawdown*100:.2f}%")
print(f"夏普比率：{result.sharpe_ratio:.2f}")
```

### 3. 手动选股

```python
from strategies.stock_selector import StockSelector

config = {
    'fundamental_factors': {
        'pe_ratio': {'enabled': True, 'min': 0, 'max': 30},
        'roe': {'enabled': True, 'min': 10}
    },
    'stock_pool': {
        'min_market_cap': 50,
        'exclude_st': True
    }
}

selector = StockSelector(config)
stocks = await selector.select_stocks(method='multi_factor', top_n=10)

for stock in stocks:
    print(f"{stock.rank}. {stock.symbol} - 评分：{stock.total_score}")
```

---

## ⚠️ 风险提示

1. **市场风险**：量化交易不能保证盈利，可能存在亏损
2. **技术风险**：网络中断、系统故障可能导致交易失败
3. **合规风险**：确保策略符合监管要求
4. **操作风险**：实盘前务必充分回测和模拟

**建议：**
- 先用模拟账户测试
- 从小资金开始实盘
- 设置严格的止损
- 定期审查策略表现

---

## 📞 获取帮助

### 遇到问题？

1. **查看日志**
   ```bash
   # 日志文件位置
   logs/trading_YYYYMMDD.log
   ```

2. **常见问题**
   - 连接失败 → 检查 QMT 终端是否运行
   - 下单被拒 → 检查资金/持仓是否充足
   - 行情延迟 → 检查网络连接

3. **联系方式**
   - 国金证券客服：95310
   - API 技术支持：联系客户经理获取
   - 项目问题：查看代码注释

---

## 📚 进阶学习

- [API_GUIDE.md](./API_GUIDE.md) - API 详细使用指南
- [README.md](./README.md) - 项目总体说明
- 官方文档：联系国金证券获取

---

**祝交易顺利！📈**
