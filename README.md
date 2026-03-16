# AIEX — AI 智能交易所

> AI 驱动的下一代加密货币交易应用

## 文件目录

```
AIEX/
├── README.md                          ← 本文件
│
├── AIEX_产品设计文档.md                 ← 📋 核心产品设计 (12章 + 附录)
├── AIEX_实施计划.md                     ← 🗓️ 8阶段全栈实施计划
├── 竞品分析与商业模式分析.md              ← 📊 竞品分析 & 商业模式
│
├── AIEX_Landing_Page.html              ← 🌐 营销着陆页 (响应式)
├── AIEX_Landing_Page_PRD.md            ← 📝 Landing Page 需求与设计详述
├── AIEX_App_Prototype.html             ← 📱 高保真交互原型 (4 Tab + Nova)
├── AIEX_App_Prototype_PRD.md           ← 📝 App 原型需求与设计详述
│
├── AIEX_Web_Prototype.html             ← 🖥️ Web 版高保真原型 (桌面端)
├── AIEX_Web_PRD.md                     ← 📝 Web 版需求与设计详述
│
├── wireframes/                         ← 🎨 Wireframe 设计稿
│   ├── 01-information-architecture.html   信息架构总览
│   ├── 02-smart-home-wireframe.html       智能首页
│   ├── 03-trading-wireframe.html          交易页 (现货+合约)
│   ├── 04-copy-trading-wireframe.html     跟单广场
│   ├── 05-ai-strategy-paywall.html        AI策略付费分层
│   └── 06-ai-overlay.html                 Nova AI 对话浮层
│
└── old_versions/                       ← 📦 旧版本 (Cowork 产出)
    ├── AI_Exchange_Architecture.html
    ├── Toobit_AI_App_v3.html
    ├── Toobit_AI_Landing_Page.html
    ├── Toobit_AI_PRD_v3.md
    └── Toobit_AI_产品方案_v2.docx
```

## 产品概要

- **产品名**: AIEX
- **AI 助手**: Nova
- **定位**: 独立品牌 AI-first 加密交易 App，使用 Toobit 底层交易能力
- **核心架构**: 4-Tab (智能首页 | 交易 | 跟单广场 | 我的) + Nova AI 全局浮层
- **目标用户**: 加密新手 + 有经验交易者
- **商业模式**: Freemium 订阅 (Free / PRO $9.9 / ELITE $29.9 / MAX $99.9)
- **参考产品**: RockFlow (Bobby AI), Cryptohopper (策略市场)

## 核心差异化

1. **Nova AI 无处不在** — 不是独立 Tab，而是融入每个交易环节的全局浮层
2. **AI 策略付费分层** — 免费策略引流 → 高级策略付费，跟单广场是订阅转化核心场景
3. **Toobit 内部 API** — 毫秒级延迟，无需用户手动配置 API Key
4. **渐进式 AI 授权** — L1 只读 → L2 确认执行 → L3 自动+通知 → L4 全自动

## 实施路线图

| Phase | 内容 | 预估时间 |
|-------|------|---------|
| 0 | 高保真交互原型 | 1-2 周 |
| 1 | 基础设施 & 脚手架 | 2 周 |
| 2 | 账户 & 认证 | 1-2 周 |
| 3 | 行情 & 交易核心 | 3-4 周 |
| 4 | 跟单广场 | 2-3 周 |
| 5 | Nova AI 服务 | 3-4 周 |
| 6 | 订阅 & 变现 | 1-2 周 |
| 7 | 智能首页 & 整合 | 2 周 |
| 8 | 发布准备 | 1-2 周 |

**总计: 16-22 周 (4-5.5 个月)**

## 技术栈

- **前端**: React Native, TypeScript, TradingView Charts
- **后端**: Go (Gin), PostgreSQL, Redis, Kafka, ClickHouse
- **AI**: Python (FastAPI), Claude API, RAG, Pinecone
- **集成**: Toobit OAuth + 内部 API
