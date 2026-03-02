# AI Trading Terminal - 完整部署指南

## 🎯 项目概述

AI Trade Terminal 是一个基于 BNB Chain 的智能交易终端，用户可以在平台上：
- 创建自定义 AI 交易策略
- 配置止盈止损参数
- 使用 AI 代理自动交易
- 实时查看交易收益

## 📁 项目结构

```
ai-trade-terminal/
├── index.html          # 前端网站
└── README.md           # 本指南

ai-trading-contracts/
├── src/
│   └── AITradingContract.sol    # 智能合约
├── test/
│   └── AITradingContract.t.sol  # 测试文件
├── script/
│   └── DeployAITrading.s.sol    # 部署脚本
├── deploy.js             # Node.js 部署脚本
├── foundry.toml          # Foundry 配置
└── README.md             # 合约文档
```

## 🔧 前置要求

- Node.js 16+
- Foundry (forge)
- BNB Chain 钱包（有 BNB 支付 Gas）

## 🚀 部署步骤

### 第一步：部署智能合约

#### 方式 1: 使用 Foundry (推荐)

```bash
# 克隆合约仓库
git clone https://github.com/Doufuru1/ai-trading-contracts.git
cd ai-trading-contracts

# 安装依赖
forge install

# 设置私钥环境变量
export PRIVATE_KEY="0xa98a4e5867019ef549591d5d7d069d1b24d7bdb5b13713fd91ef2dc593375cfd"

# 部署到 BNB Chain 主网
forge create src/AITradingContract.sol:AITradingContract \
  --rpc-url https://bsc-dataseed.binance.org \
  --private-key $PRIVATE_KEY \
  --constructor-args 0x768e55A759A660f014C788a0E941b843bD23727b \
  --legacy \
  --broadcast
```

#### 方式 2: 使用 Node.js

```bash
cd ai-trading-contracts

# 安装依赖
npm install

# 运行部署脚本
node deploy.js
```

#### 方式 3: 使用 Remix

1. 打开 [Remix IDE](https://remix.ethereum.org)
2. 新建文件 `AITradingContract.sol`，粘贴合约代码
3. 编译合约 (Solidity 0.8.20+)
4. 连接 MetaMask (BNB Chain 网络)
5. 部署时传入参数: `0x768e55A759A660f014C788a0E941b843bD23727b`

### 第二步：记录合约地址

部署成功后，你会看到类似输出：

```
Contract Address: 0x1234567890abcdef1234567890abcdef12345678
Transaction Hash: 0xabcdef...
```

**保存这个地址！**

### 第三步：更新网站配置

1. 打开 `ai-trade-terminal/index.html`
2. 找到 `CONTRACT_ADDRESS` 配置：

```javascript
// 替换为你的实际合约地址
const CONTRACT_ADDRESS = '0xYOUR_DEPLOYED_CONTRACT_ADDRESS';

// 关闭演示模式
const DEMO_MODE = false;
```

3. 保存文件

### 第四步：重新部署网站

```bash
cd ai-trade-terminal

# 提交更新
git add index.html
git commit -m "Update contract address"
git push origin main

# 重新部署到 Vercel
vercel --prod
```

## 📋 合约功能

### 用户功能

| 功能 | 说明 |
|------|------|
| `createStrategy()` | 创建交易策略，投入 BNB |
| `executeTrade()` | 执行买卖交易 |
| `toggleStrategy()` | 暂停/恢复策略 |
| `stopStrategy()` | 停止策略并提取资金 |
| `createAIAgent()` | 创建 AI 代理 |

### 策略类型

- **0**: 趋势跟踪 (Trend Following)
- **1**: 套利 (Arbitrage)
- **2**: 网格交易 (Grid Trading)
- **3**: 动量交易 (Momentum)
- **4**: 均值回归 (Mean Reversion)

## 🌐 网站访问

部署完成后：

- **网站**: https://ai-trade-terminal.vercel.app
- **合约**: 你的合约地址
- **BSCScan**: https://bscscan.com/address/[你的合约地址]

## 🎮 使用方法

### 1. 连接钱包
- 点击右上角 "连接钱包"
- 选择 MetaMask
- 确保在 BNB Chain 网络

### 2. 创建策略
- 填写策略名称
- 选择策略类型
- 选择交易代币对
- 设置止盈止损 (%)
- 输入投资金额 (BNB)
- 点击 "创建策略"

### 3. 查看收益
- 在 AI 代理卡片查看收益
- 实时查看交易记录
- 查看总资产变化

## ⚠️ 注意事项

1. **Gas 费用**: 部署和交易需要 BNB 支付 Gas
2. **最低投资**: 0.01 BNB
3. **平台费用**: 盈利部分收取 1% 手续费
4. **风险提示**: 加密货币交易有风险，请谨慎投资

## 🔒 安全建议

- 私钥不要泄露
- 合约代码建议审计后再使用
- 小额测试后再大额投资
- 定期检查策略状态

## 📞 技术支持

如有问题，请查看：
- GitHub Issues: https://github.com/Doufuru1/ai-trading-contracts/issues
- GitHub Discussions: https://github.com/Doufuru1/ai-trading-contracts/discussions

## 📄 许可证

MIT License
