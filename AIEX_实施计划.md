# AIEX 全栈实施计划

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 从零到一构建 AIEX — AI 智能交易所的完整 MVP，覆盖高保真原型、React Native 前端、Go 后端、Python AI 服务。

**Architecture:** 4 层架构 — React Native 移动端 → Go API Gateway → 微服务集群（交易/跟单/订阅/用户） → Python AI 服务（Nova 引擎）。Toobit 作为底层交易基础设施通过内部 API 集成。

**Tech Stack:** React Native, TypeScript, Go (Gin), Python (FastAPI), PostgreSQL, Redis, Kafka, ClickHouse, Claude API, TradingView Lightweight Charts, WebSocket

**Spec:** `docs/superpowers/specs/2026-03-16-aiex-product-design.md`

---

## 总体路线图

```
Phase 0: 高保真交互原型 (1-2 周)
  └─ 可演示的完整 App 原型，用于团队评审和投资人展示

Phase 1: 基础设施 & 脚手架 (2 周)
  ├─ React Native 项目初始化 + 导航架构
  ├─ Go 后端项目初始化 + API Gateway
  ├─ PostgreSQL 数据库 Schema
  └─ CI/CD 管道

Phase 2: 账户 & 认证 (1-2 周)
  ├─ Toobit OAuth 集成
  ├─ 用户服务 (注册/登录/Token管理)
  ├─ Onboarding 流程
  └─ 风险偏好设置

Phase 3: 行情 & 交易核心 (3-4 周)
  ├─ Toobit 行情 API 代理 + WebSocket
  ├─ 现货交易服务
  ├─ 合约交易服务
  ├─ 前端交易页面 (现货/合约)
  └─ K 线图 (TradingView)

Phase 4: 跟单广场 (2-3 周)
  ├─ 跟单服务 (带单者/跟单者/分润)
  ├─ AI 策略管理服务
  ├─ 前端跟单广场页面
  └─ 高水位分润结算

Phase 5: Nova AI 服务 (3-4 周)
  ├─ Nova 引擎 (Python FastAPI)
  ├─ Claude API 集成 + RAG
  ├─ 自然语言下单解析 (NLU)
  ├─ 市场分析 & 持仓建议生成
  ├─ 前端 Nova 对话浮层
  └─ 上下文感知 + 行动卡片

Phase 6: 订阅 & 变现 (1-2 周)
  ├─ 订阅服务 (4 档会员)
  ├─ 支付集成
  ├─ 付费墙 & 策略解锁逻辑
  └─ Nova 对话计量

Phase 7: 智能首页 & 整合 (2 周)
  ├─ 智能首页信息流
  ├─ AI 市场快报生成
  ├─ 持仓 AI 建议
  ├─ 断路器机制
  └─ 全流程集成测试

Phase 8: 发布准备 (1-2 周)
  ├─ i18n (中/英)
  ├─ 性能优化
  ├─ 安全审计
  ├─ App Store / Google Play 上架准备
  └─ Landing Page
```

**总计预估: 16-22 周 (4-5.5 个月)**

---

## Phase 0: 高保真交互原型

**目标:** 一个完整可交互的 App 原型（单一 HTML 文件），涵盖所有 4 个 Tab + Nova 对话浮层 + Onboarding。用于团队评审和投资人展示。

**为什么先做原型:** 现有原型（Cowork 产出）存在两套互相矛盾的概念、缺少 Onboarding、缺少付费分层 UI、Nova 对话是硬编码。新原型需要完整体现设计文档的所有决策。

### Task 0.1: 项目搭建 & 设计系统

**Files:**
- Create: `prototype/aiex-prototype.html`

- [ ] **Step 1: 创建 HTML 骨架**

创建单文件 HTML，包含：
- CSS 变量定义（颜色、字体、圆角、阴影）
- 全局 reset 和暗色主题基础样式
- iPhone 手机外框 (390x844)
- 4-Tab 底部导航栏
- Tab 切换 JavaScript

设计系统变量：
```css
:root {
  --bg-primary: #0a0a1a;
  --bg-card: #12122a;
  --bg-input: #1a1a3a;
  --border: #2a2a4a;
  --text-primary: #ffffff;
  --text-secondary: #888888;
  --accent-purple: #a78bfa;
  --accent-green: #34d399;
  --accent-red: #ef4444;
  --accent-gold: #f0c040;
  --accent-blue: #60a5fa;
  --accent-pink: #f472b6;
  --radius-sm: 8px;
  --radius-md: 12px;
  --radius-lg: 16px;
  --radius-pill: 24px;
}
```

- [ ] **Step 2: 验证 Tab 切换功能**

在浏览器中打开，确认 4 个 Tab 可以切换，内容区域正确显示/隐藏。

- [ ] **Step 3: Commit**

```bash
git add prototype/aiex-prototype.html
git commit -m "feat: prototype scaffold with design system and 4-tab navigation"
```

### Task 0.2: Onboarding 流程

**Files:**
- Modify: `prototype/aiex-prototype.html`

- [ ] **Step 1: 实现 3 屏引导**

启动时显示全屏 Onboarding overlay：
- 第 1 屏：Nova AI 助手介绍 + 动画
- 第 2 屏：跟单广场介绍
- 第 3 屏：AI 策略介绍
- 滑动指示器（3 个点）
- "开始使用" 按钮

- [ ] **Step 2: 实现 Toobit OAuth 模拟**

点击"开始使用" → 模拟 Toobit 登录页面 → 点击"授权登录" → 进入风险偏好设置

- [ ] **Step 3: 风险偏好选择**

3 个选项卡片：保守 / 稳健 / 激进
选择后 → Nova 欢迎对话弹出 → 关闭后进入主 App

- [ ] **Step 4: 验证完整 Onboarding 流程**

- [ ] **Step 5: Commit**

```bash
git commit -m "feat: onboarding flow with 3-screen guide, mock OAuth, risk preference"
```

### Task 0.3: 智能首页

**Files:**
- Modify: `prototype/aiex-prototype.html`

- [ ] **Step 1: 资产卡片**

渐变背景卡片，显示总资产、今日盈亏、迷你 SVG 收益曲线。

- [ ] **Step 2: AI 市场快报**

3 张洞察卡片（风险红/机会绿/洞察紫），可点击展开详情。

- [ ] **Step 3: 持仓 AI 建议**

基于 BTC 持仓的 Nova 建议卡片，带"执行止盈"和"稍后再看"按钮。按钮点击有视觉反馈。

- [ ] **Step 4: 热门策略横滑**

3+ 张策略卡片（AI 策略 + 真人实盘混排），水平滚动。

- [ ] **Step 5: 行情概览**

涨幅榜/跌幅榜/成交量 3 个子 Tab，7+ 个币种行情数据。

- [ ] **Step 6: Nova 输入框**

底部常驻输入框，点击后弹出 Nova 对话浮层（Task 0.7 实现）。

- [ ] **Step 7: Commit**

```bash
git commit -m "feat: smart homepage with AI briefing, position advice, strategies, market overview"
```

### Task 0.4: 交易页 — 现货

**Files:**
- Modify: `prototype/aiex-prototype.html`

- [ ] **Step 1: 交易页框架**

交易对选择器、现货/合约 Tab 切换、Nova 自然语言下单栏。

- [ ] **Step 2: 价格 & K 线区域**

当前价格显示（大字体绿/红色）、24H 高低量、SVG 模拟 K 线图、时间周期切换按钮。

- [ ] **Step 3: Nova 分析条**

K 线下方一行 AI 分析摘要，点击展开详情面板。

- [ ] **Step 4: 下单面板**

买入/卖出切换、限价/市价/止盈止损、价格和数量输入框、25%/50%/75%/100% 快捷按钮。

- [ ] **Step 5: Nova 建议卡片**

下单面板内的 AI 建议（止损位、仓位建议），"自动设置止损"按钮。

- [ ] **Step 6: 买入/卖出按钮**

点击后显示订单确认弹窗（模拟），确认后显示成功 toast。

- [ ] **Step 7: Commit**

```bash
git commit -m "feat: spot trading page with AI analysis bar and order panel"
```

### Task 0.5: 交易页 — 合约

**Files:**
- Modify: `prototype/aiex-prototype.html`

- [ ] **Step 1: 合约模式切换**

切换到合约 Tab 后显示：开多/开空、逐仓/全仓、杠杆倍数。

- [ ] **Step 2: 杠杆滑块**

1x-125x 滑块，拖动时动态更新：风险等级颜色、预估强平价。

- [ ] **Step 3: Nova 风控评估面板**

红色边框面板：风险等级、强平价、建议止损、建议止盈。"采纳 AI 建议"按钮点击后自动填充参数。

- [ ] **Step 4: Commit**

```bash
git commit -m "feat: futures trading with leverage slider and AI risk assessment"
```

### Task 0.6: 跟单广场

**Files:**
- Modify: `prototype/aiex-prototype.html`

- [ ] **Step 1: 实盘交易者列表**

3 个子 Tab（实盘交易者/AI策略/我的跟单）、筛选排序栏、时间筛选。2-3 个交易者卡片（等级、数据、收益曲线）。

- [ ] **Step 2: AI 策略分层**

免费策略（2 个，绿色"跟单"按钮）+ 高级策略（3 个，带🔒标签和"升级解锁"按钮）。

- [ ] **Step 3: 付费墙弹窗**

点击高级策略"解锁" → 弹出订阅选择页：4 档会员对比表 + 价格 + CTA。

- [ ] **Step 4: Nova 智能推荐入口**

底部渐变卡片："不知道跟谁？让 Nova 帮你选"，点击唤起 Nova。

- [ ] **Step 5: 我的跟单 Tab**

已跟单列表（模拟数据），显示实时收益、跟单状态。

- [ ] **Step 6: Commit**

```bash
git commit -m "feat: copy trading plaza with free/paid strategy tiers and paywall"
```

### Task 0.7: Nova AI 对话浮层

**Files:**
- Modify: `prototype/aiex-prototype.html`

- [ ] **Step 1: 浮层 UI**

半屏浮层：拖动条、"Nova · 上下文标签"头部、对话内容区、底部输入框 + 快捷 Chips。
支持手势：向上拖全屏、向下拖收起。

- [ ] **Step 2: 模拟对话引擎**

基于关键词路由的模拟 AI 回复：
- 包含 "BTC" / "比特币" → 返回 BTC 分析 + 行动卡片（建议订单）
- 包含 "持仓" / "portfolio" → 返回持仓分析
- 包含 "推荐" / "策略" → 返回策略推荐卡片
- 默认 → 返回通用市场概览

- [ ] **Step 3: 行动卡片**

下单预览卡片（方向/数量/价格/止损止盈 + 确认/修改按钮）。点击"确认下单"显示成功 toast。

- [ ] **Step 4: 上下文感知**

根据当前活跃 Tab 显示不同的上下文标签和快捷 Chips：
- 交易页 → "BTC/USDT 交易页" + [下单, 设止损, 分析]
- 首页 → "智能首页" + [今日热点, 持仓分析, 推荐策略]
- 跟单 → "跟单广场" + [帮我选, 分析交易者, 风险评估]

- [ ] **Step 5: 全局 Nova 输入框**

所有 4 个 Tab 底部都有 Nova 输入框，点击唤起浮层。

- [ ] **Step 6: Commit**

```bash
git commit -m "feat: Nova AI overlay with context-aware responses and action cards"
```

### Task 0.8: 我的页面

**Files:**
- Modify: `prototype/aiex-prototype.html`

- [ ] **Step 1: 资产总览**

总资产卡片 + 眼睛隐藏切换 + 盈亏曲线。

- [ ] **Step 2: 菜单列表**

交易历史、AI 订阅管理、Nova 授权等级、账户绑定、设置、帮助、关于。

- [ ] **Step 3: 订阅管理弹窗**

点击"AI 订阅管理" → 弹出订阅页面：4 档会员卡片、月/年切换、当前等级标记。

- [ ] **Step 4: Nova 授权等级设置**

点击"Nova 授权等级" → 弹出 L1-L4 选择页面，每级有说明和风险提示。

- [ ] **Step 5: Commit**

```bash
git commit -m "feat: profile page with subscription management and Nova authorization levels"
```

### Task 0.9: 最终打磨 & 交互完善

**Files:**
- Modify: `prototype/aiex-prototype.html`

- [ ] **Step 1: 页面转场动画**

Tab 切换渐入效果、弹窗滑入效果、卡片点击反馈。

- [ ] **Step 2: Loading 状态模拟**

首次加载时显示骨架屏（skeleton），模拟真实数据加载体验。

- [ ] **Step 3: 风险提示**

合约交易页底部固定风险提示条、首次打开合约交易弹窗警告。

- [ ] **Step 4: 整体检查**

遍历所有页面和交互，确保无断裂的跳转、无空白页面、所有按钮有反馈。

- [ ] **Step 5: Commit**

```bash
git commit -m "feat: polish prototype with animations, skeleton loading, risk disclaimers"
```

---

## Phase 1: 基础设施 & 脚手架

**目标:** 搭建真实项目的技术基础，包括前端 React Native 项目、后端 Go 项目、数据库 Schema、CI/CD。

### Task 1.1: React Native 项目初始化

**Files:**
- Create: `mobile/` — React Native 项目根目录
- Create: `mobile/src/navigation/TabNavigator.tsx`
- Create: `mobile/src/theme/tokens.ts`
- Create: `mobile/src/components/NovaInput.tsx`

- [ ] **Step 1: 初始化 React Native 项目**

```bash
npx react-native@latest init AIEX --template react-native-template-typescript
mv AIEX mobile
cd mobile
npm install @react-navigation/native @react-navigation/bottom-tabs react-native-screens react-native-safe-area-context
```

- [ ] **Step 2: 创建设计 Token 文件**

`mobile/src/theme/tokens.ts` — 与原型 CSS 变量一致的 TypeScript 常量。

- [ ] **Step 3: 创建 4-Tab 导航器**

`mobile/src/navigation/TabNavigator.tsx` — 智能首页/交易/跟单广场/我的，暗色主题。

- [ ] **Step 4: 创建全局 Nova 输入框组件**

`mobile/src/components/NovaInput.tsx` — 底部常驻输入框，所有 Tab 可见。

- [ ] **Step 5: 验证 App 启动和 Tab 切换**

```bash
cd mobile && npx react-native run-ios
```

- [ ] **Step 6: Commit**

```bash
git commit -m "feat: React Native project with 4-tab navigation and design tokens"
```

### Task 1.2: Go 后端项目初始化

**Files:**
- Create: `backend/` — Go 项目根目录
- Create: `backend/cmd/api/main.go`
- Create: `backend/internal/config/config.go`
- Create: `backend/internal/middleware/auth.go`
- Create: `backend/internal/middleware/ratelimit.go`
- Create: `backend/api/router.go`

- [ ] **Step 1: 初始化 Go module**

```bash
mkdir -p backend && cd backend
go mod init github.com/toobit/aiex-backend
go get github.com/gin-gonic/gin
go get github.com/jackc/pgx/v5
go get github.com/redis/go-redis/v9
```

- [ ] **Step 2: 创建 API 入口和路由**

`backend/cmd/api/main.go` — Gin 服务启动，加载配置，注册路由。
`backend/api/router.go` — 路由分组：`/api/v1/auth`, `/api/v1/market`, `/api/v1/trade`, `/api/v1/copy`, `/api/v1/subscription`, `/api/v1/nova`。

- [ ] **Step 3: 创建中间件**

JWT 认证中间件、请求限流中间件（基于 Redis）。

- [ ] **Step 4: 健康检查端点**

`GET /api/v1/health` 返回 `{"status": "ok", "version": "0.1.0"}`。

- [ ] **Step 5: 写测试并验证**

```bash
cd backend && go test ./... -v
```

- [ ] **Step 6: Commit**

```bash
git commit -m "feat: Go backend scaffold with Gin router, auth middleware, health check"
```

### Task 1.3: 数据库 Schema

**Files:**
- Create: `backend/migrations/001_initial_schema.sql`

- [ ] **Step 1: 设计核心表**

```sql
-- 用户表
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  toobit_user_id VARCHAR(64) UNIQUE NOT NULL,
  nickname VARCHAR(64),
  avatar_url TEXT,
  risk_preference VARCHAR(16) DEFAULT 'moderate', -- conservative/moderate/aggressive
  nova_auth_level INT DEFAULT 1, -- 1-4
  subscription_tier VARCHAR(16) DEFAULT 'free', -- free/pro/elite/max
  subscription_expires_at TIMESTAMPTZ,
  language VARCHAR(8) DEFAULT 'zh',
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- 订阅记录
CREATE TABLE subscriptions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id),
  tier VARCHAR(16) NOT NULL,
  price_cents INT NOT NULL,
  started_at TIMESTAMPTZ NOT NULL,
  expires_at TIMESTAMPTZ NOT NULL,
  payment_method VARCHAR(32),
  status VARCHAR(16) DEFAULT 'active', -- active/cancelled/expired
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- 跟单关系
CREATE TABLE copy_trades (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  follower_id UUID REFERENCES users(id),
  leader_id UUID, -- NULL if AI strategy
  strategy_id UUID, -- NULL if human leader
  copy_amount DECIMAL(20,8) NOT NULL,
  profit_share_rate DECIMAL(5,4) DEFAULT 0.15,
  high_water_mark DECIMAL(20,8) DEFAULT 0,
  cumulative_profit DECIMAL(20,8) DEFAULT 0,
  status VARCHAR(16) DEFAULT 'active', -- active/paused/stopped
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- AI 策略
CREATE TABLE ai_strategies (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR(128) NOT NULL,
  name_en VARCHAR(128),
  description TEXT,
  description_en TEXT,
  strategy_type VARCHAR(32) NOT NULL, -- grid/dca/trend/alpha/arbitrage
  risk_level VARCHAR(16) NOT NULL, -- low/medium/high
  min_tier VARCHAR(16) DEFAULT 'free', -- free/pro/elite/max
  is_active BOOLEAN DEFAULT true,
  config JSONB,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- AI 策略业绩快照（每日）
CREATE TABLE strategy_snapshots (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  strategy_id UUID REFERENCES ai_strategies(id),
  snapshot_date DATE NOT NULL,
  roi_7d DECIMAL(10,4),
  roi_30d DECIMAL(10,4),
  roi_90d DECIMAL(10,4),
  win_rate DECIMAL(5,4),
  max_drawdown DECIMAL(5,4),
  sharpe_ratio DECIMAL(6,3),
  follower_count INT DEFAULT 0,
  UNIQUE(strategy_id, snapshot_date)
);

-- Nova 对话历史
CREATE TABLE nova_conversations (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id),
  context_type VARCHAR(32), -- home/trade/copy/profile
  context_data JSONB, -- e.g. {"symbol": "BTCUSDT", "tab": "spot"}
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE nova_messages (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  conversation_id UUID REFERENCES nova_conversations(id),
  role VARCHAR(16) NOT NULL, -- user/assistant
  content TEXT NOT NULL,
  action_card JSONB, -- structured action card data
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Nova 每日用量计数
CREATE TABLE nova_daily_usage (
  user_id UUID REFERENCES users(id),
  usage_date DATE NOT NULL,
  message_count INT DEFAULT 0,
  PRIMARY KEY (user_id, usage_date)
);

-- 带单者
CREATE TABLE leaders (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id) UNIQUE,
  display_name VARCHAR(64) NOT NULL,
  bio TEXT,
  tier VARCHAR(16) DEFAULT 'bronze', -- bronze/silver/gold
  style_tags JSONB, -- ["合约专家", "稳健风格"]
  profit_share_rate DECIMAL(5,4) DEFAULT 0.15,
  is_active BOOLEAN DEFAULT true,
  total_followers INT DEFAULT 0,
  trading_days INT DEFAULT 0,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- 分润结算记录
CREATE TABLE profit_settlements (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  copy_trade_id UUID REFERENCES copy_trades(id),
  settlement_period_start DATE NOT NULL,
  settlement_period_end DATE NOT NULL,
  period_profit DECIMAL(20,8) NOT NULL,
  high_water_mark_before DECIMAL(20,8) NOT NULL,
  high_water_mark_after DECIMAL(20,8) NOT NULL,
  settlement_amount DECIMAL(20,8) NOT NULL, -- actual fee charged
  status VARCHAR(16) DEFAULT 'pending', -- pending/settled/failed
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

- [ ] **Step 2: 创建索引**

```sql
CREATE INDEX idx_copy_trades_follower ON copy_trades(follower_id, status);
CREATE INDEX idx_copy_trades_leader ON copy_trades(leader_id, status);
CREATE INDEX idx_nova_conversations_user ON nova_conversations(user_id, created_at DESC);
CREATE INDEX idx_nova_messages_conversation ON nova_messages(conversation_id, created_at);
CREATE INDEX idx_strategy_snapshots_date ON strategy_snapshots(strategy_id, snapshot_date DESC);
CREATE INDEX idx_nova_daily_usage_date ON nova_daily_usage(usage_date);
```

- [ ] **Step 3: 验证 migration**

```bash
psql -d aiex_dev -f backend/migrations/001_initial_schema.sql
```

- [ ] **Step 4: Commit**

```bash
git commit -m "feat: initial database schema for users, subscriptions, copy trading, Nova"
```

### Task 1.4: Python AI 服务初始化

**Files:**
- Create: `ai-service/` — Python 项目根目录
- Create: `ai-service/pyproject.toml`
- Create: `ai-service/src/main.py`
- Create: `ai-service/src/nova/engine.py`
- Create: `ai-service/src/nova/context.py`

- [ ] **Step 1: 初始化 Python 项目**

```bash
mkdir -p ai-service/src/nova
cd ai-service
# pyproject.toml with: fastapi, uvicorn, anthropic, redis, asyncpg
pip install -e ".[dev]"
```

- [ ] **Step 2: FastAPI 入口**

`ai-service/src/main.py` — 启动 FastAPI，注册路由：
- `POST /api/v1/nova/chat` — Nova 对话
- `GET /api/v1/nova/briefing` — AI 市场快报
- `GET /api/v1/nova/position-advice/{user_id}` — 持仓建议

- [ ] **Step 3: Nova 引擎骨架**

`ai-service/src/nova/engine.py` — NovaEngine 类，接受消息和上下文，调用 Claude API，返回结构化回复（含 action_card）。

- [ ] **Step 4: 健康检查**

`GET /health` 返回 `{"status": "ok"}`。

- [ ] **Step 5: Commit**

```bash
git commit -m "feat: Python AI service scaffold with FastAPI and Nova engine skeleton"
```

### Task 1.5: Docker Compose 本地开发环境

**Files:**
- Create: `docker-compose.yml`
- Create: `backend/Dockerfile`
- Create: `ai-service/Dockerfile`

- [ ] **Step 1: 编写 docker-compose.yml**

服务：postgres, redis, backend (Go), ai-service (Python), kafka, clickhouse。

- [ ] **Step 2: 验证所有服务启动**

```bash
docker-compose up -d
docker-compose ps  # 确认所有服务 healthy
curl http://localhost:8080/api/v1/health  # Go backend
curl http://localhost:8000/health  # Python AI service
```

- [ ] **Step 3: Commit**

```bash
git commit -m "feat: Docker Compose for local development with all services"
```

---

## Phase 2: 账户 & 认证

### Task 2.1: Toobit OAuth 服务端

**Files:**
- Create: `backend/internal/service/auth_service.go`
- Create: `backend/internal/handler/auth_handler.go`
- Create: `backend/internal/model/user.go`
- Test: `backend/internal/service/auth_service_test.go`

- [ ] **Step 1: 写 Auth Service 测试**

测试 Toobit OAuth token 交换、用户创建/查找、JWT 生成。

- [ ] **Step 2: 实现 Auth Service**

- `ExchangeOAuthCode(code string) → ToobitTokens` — 调用 Toobit OAuth API
- `FindOrCreateUser(toobitUserID string) → User` — 查找或创建用户
- `GenerateJWT(user User) → string` — 生成 AIEX JWT
- `RefreshToken(refreshToken string) → Tokens`

- [ ] **Step 3: 实现 Auth Handler**

- `POST /api/v1/auth/toobit/callback` — OAuth 回调
- `POST /api/v1/auth/refresh` — 刷新 Token
- `GET /api/v1/auth/me` — 当前用户信息

- [ ] **Step 4: 运行测试**

- [ ] **Step 5: Commit**

### Task 2.2: React Native 登录 & Onboarding

**Files:**
- Create: `mobile/src/screens/onboarding/OnboardingScreen.tsx`
- Create: `mobile/src/screens/onboarding/LoginScreen.tsx`
- Create: `mobile/src/screens/onboarding/RiskPreferenceScreen.tsx`
- Create: `mobile/src/services/auth.ts`
- Create: `mobile/src/stores/authStore.ts`

- [ ] **Step 1: Onboarding 3 屏引导页**
- [ ] **Step 2: Toobit OAuth WebView 登录**
- [ ] **Step 3: 风险偏好选择页**
- [ ] **Step 4: Token 存储和自动刷新**
- [ ] **Step 5: 验证完整登录流程**
- [ ] **Step 6: Commit**

---

## Phase 3: 行情 & 交易核心

### Task 3.1: Toobit 行情代理服务

**Files:**
- Create: `backend/internal/service/market_service.go`
- Create: `backend/internal/handler/market_handler.go`
- Create: `backend/internal/ws/market_ws.go`

端点：
- `GET /api/v1/market/tickers` — 所有交易对行情
- `GET /api/v1/market/klines/{symbol}` — K线数据
- `WS /api/v1/market/ws` — WebSocket 实时推送

### Task 3.2: 现货交易服务

**Files:**
- Create: `backend/internal/service/spot_service.go`
- Create: `backend/internal/handler/spot_handler.go`

端点：
- `POST /api/v1/trade/spot/order` — 下单
- `DELETE /api/v1/trade/spot/order/{id}` — 撤单
- `GET /api/v1/trade/spot/orders` — 订单列表
- `GET /api/v1/trade/spot/positions` — 持仓

### Task 3.3: 合约交易服务

**Files:**
- Create: `backend/internal/service/futures_service.go`
- Create: `backend/internal/handler/futures_handler.go`

端点：
- `POST /api/v1/trade/futures/order` — 开仓/平仓
- `PUT /api/v1/trade/futures/leverage` — 设置杠杆
- `PUT /api/v1/trade/futures/margin-mode` — 逐仓/全仓切换
- `GET /api/v1/trade/futures/positions` — 持仓

### Task 3.4: 前端交易页面

**Files:**
- Create: `mobile/src/screens/trade/TradeScreen.tsx`
- Create: `mobile/src/screens/trade/SpotOrderPanel.tsx`
- Create: `mobile/src/screens/trade/FuturesOrderPanel.tsx`
- Create: `mobile/src/screens/trade/LeverageSlider.tsx`
- Create: `mobile/src/components/charts/MiniChart.tsx`
- Create: `mobile/src/components/nova/NovaAnalysisBar.tsx`
- Create: `mobile/src/components/nova/NovaRiskPanel.tsx`

---

## Phase 4: 跟单广场

### Task 4.1: 跟单后端服务

**Files:**
- Create: `backend/internal/service/copy_service.go`
- Create: `backend/internal/handler/copy_handler.go`
- Create: `backend/internal/service/settlement_service.go`

端点：
- `GET /api/v1/copy/leaders` — 带单者排行
- `GET /api/v1/copy/strategies` — AI 策略列表
- `POST /api/v1/copy/follow` — 跟单
- `DELETE /api/v1/copy/follow/{id}` — 取消跟单
- `GET /api/v1/copy/my` — 我的跟单

结算逻辑：每 7 天运行 `SettlementService.RunWeeklySettlement()`，遍历所有 active copy_trades，计算高水位分润。

### Task 4.2: 前端跟单广场

**Files:**
- Create: `mobile/src/screens/copy/CopyTradingScreen.tsx`
- Create: `mobile/src/screens/copy/LeaderCard.tsx`
- Create: `mobile/src/screens/copy/StrategyCard.tsx`
- Create: `mobile/src/screens/copy/PaywallModal.tsx`
- Create: `mobile/src/screens/copy/MyCopyTrades.tsx`

---

## Phase 5: Nova AI 服务

### Task 5.1: Nova 对话引擎

**Files:**
- Modify: `ai-service/src/nova/engine.py`
- Create: `ai-service/src/nova/prompts.py`
- Create: `ai-service/src/nova/tools.py`
- Create: `ai-service/src/nova/action_cards.py`

核心逻辑：
1. 接收用户消息 + 上下文（当前页面、交易对、持仓）
2. 构造 Claude API system prompt（包含市场数据、用户持仓、授权等级）
3. 使用 Claude tool_use 功能：定义 tools（place_order, analyze_market, recommend_strategy, set_stop_loss）
4. 解析 tool_use 结果，生成 action_card JSON
5. 检查用户授权等级：L1 只返回建议、L2 返回可确认的 action_card、L3 自动执行

### Task 5.2: 自然语言下单解析 (NLU)

**Files:**
- Create: `ai-service/src/nova/nlu_parser.py`
- Test: `ai-service/tests/test_nlu_parser.py`

解析示例：
- "买入 0.1 BTC" → `{action: "buy", symbol: "BTCUSDT", quantity: 0.1, type: "market"}`
- "在 65000 挂限价单买 BTC" → `{action: "buy", symbol: "BTCUSDT", price: 65000, type: "limit"}`
- "平掉 ETH 多单" → `{action: "close", symbol: "ETHUSDT", side: "long"}`

### Task 5.3: AI 市场快报生成

**Files:**
- Create: `ai-service/src/nova/briefing.py`

每日 3 条 AI 洞察（风险/机会/洞察），基于：
- 过去 24H 价格变动
- 成交量异常
- 技术指标（RSI/MACD/布林带）

### Task 5.4: 持仓 AI 建议

**Files:**
- Create: `ai-service/src/nova/position_advisor.py`

基于用户持仓 + 市场数据，生成个性化建议：
- 止盈/止损提醒
- 仓位调整建议
- 风险预警

### Task 5.5: 前端 Nova 对话浮层

**Files:**
- Create: `mobile/src/components/nova/NovaOverlay.tsx`
- Create: `mobile/src/components/nova/NovaMessage.tsx`
- Create: `mobile/src/components/nova/ActionCard.tsx`
- Create: `mobile/src/components/nova/QuickChips.tsx`
- Create: `mobile/src/stores/novaStore.ts`
- Create: `mobile/src/services/nova.ts`

---

## Phase 6: 订阅 & 变现

### Task 6.1: 订阅后端服务

**Files:**
- Create: `backend/internal/service/subscription_service.go`
- Create: `backend/internal/handler/subscription_handler.go`
- Create: `backend/internal/middleware/subscription_gate.go`

端点：
- `GET /api/v1/subscription/plans` — 订阅方案列表
- `POST /api/v1/subscription/subscribe` — 创建订阅
- `POST /api/v1/subscription/cancel` — 取消订阅
- `GET /api/v1/subscription/current` — 当前订阅信息

门控中间件：检查用户订阅等级，决定是否允许访问高级策略、自动交易等。

### Task 6.2: Nova 对话计量

**Files:**
- Modify: `ai-service/src/nova/engine.py`
- Create: `ai-service/src/nova/rate_limiter.py`

每次 Nova 对话前检查 `nova_daily_usage`：
- Free: 10 次/日
- PRO: 100 次/日
- ELITE/MAX: 无限

### Task 6.3: 前端订阅管理

**Files:**
- Create: `mobile/src/screens/profile/SubscriptionScreen.tsx`
- Modify: `mobile/src/screens/copy/PaywallModal.tsx`

---

## Phase 7: 智能首页 & 整合

### Task 7.1: 智能首页前端

**Files:**
- Create: `mobile/src/screens/home/SmartHomeScreen.tsx`
- Create: `mobile/src/screens/home/AssetCard.tsx`
- Create: `mobile/src/screens/home/AIBriefingCard.tsx`
- Create: `mobile/src/screens/home/PositionAdviceCard.tsx`
- Create: `mobile/src/screens/home/TrendingStrategies.tsx`
- Create: `mobile/src/screens/home/MarketOverview.tsx`

### Task 7.2: 断路器机制

**Files:**
- Create: `backend/internal/service/circuit_breaker.go`
- Test: `backend/internal/service/circuit_breaker_test.go`

监控条件：
- 单币种 5min ±10%
- BTC 1h ±8%
- 用户日亏损 ≥ 总资产 20%

触发后：暂停该用户所有 L3 自动交易，发送推送通知。

### Task 7.3: 全流程集成测试

端到端测试：
1. 用户注册 → Onboarding → 设置风险偏好
2. 查看智能首页 → AI 快报 → 持仓建议
3. 现货交易 → Nova 辅助下单 → 确认
4. 合约交易 → 杠杆设置 → Nova 风控
5. 跟单广场 → 免费策略跟单 → 付费策略 paywall
6. Nova 对话 → 上下文感知 → 行动卡片
7. 订阅升级 → 解锁高级策略

---

## Phase 8: 发布准备

### Task 8.1: i18n 多语言

所有前端文案使用 i18n key，支持 zh-CN 和 en-US。Nova 根据用户语言设置调用不同 system prompt。

### Task 8.2: 性能优化

- K 线图懒加载
- 行情 WebSocket 连接池
- 图片/头像 CDN
- 列表虚拟滚动

### Task 8.3: 安全审计

- API 所有输入参数验证
- Nova 对话 XSS 防护（不直接渲染 HTML）
- JWT Token 过期和刷新
- 敏感操作二次确认（大额交易、授权等级变更）

### Task 8.4: Landing Page

基于设计文档的 Landing Page，暗色主题，包含：hero、功能介绍、定价对比（vs Cryptohopper）、安全信任、下载 CTA。

### Task 8.5: App Store 准备

- iOS: App Store 审核材料、截图、描述
- Android: Google Play 上架材料
- 隐私政策、用户协议

---

*实施计划基于 `docs/superpowers/specs/2026-03-16-aiex-product-design.md` 设计文档。Phase 0 的交互原型可以立即开始，后续 Phase 可并行推进（前端/后端/AI 服务独立开发）。*
