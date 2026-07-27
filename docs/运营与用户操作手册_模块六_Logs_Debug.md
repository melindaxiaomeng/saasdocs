# 运营与用户操作手册 · 模块六  
## Logs & Debug 日志查询与调试操作 SOP

> **适用角色**：技术运营、渠道经理（PM）、开发调试人员  
> **对应系统页面**：`Logs & Debug`（/logs）  
> **文档版本**：v1.0 · 2026-06-22  

---

## 1. 业务场景与心智

Logs & Debug 是平台的**全链路日志追踪与调试中枢**，覆盖从点击到 Postback 的完整数据流。

| 业务场景 | 说明 |
|---------|------|
| Postback 日志追踪 | 查看每次点击的 Postback 发送状态、响应内容、延迟时间 |
| 异常日志排查 | 通过 Not PB Msg 结构化展示，快速定位 Postback 失败原因 |
| PM 渠道日志 | 查看 PM（Partner Manager）维度的 Postback 日志 |
| 错误报告分析 | 聚合查看错误分布（按 Reason 分组）|
| 调试 API | 构造调试请求，验证 API 参数和响应 |
| 查询任务管理 | 管理长时间运行的查询任务，下载查询结果 |

> ⚠️ **核心概念**：Logs & Debug 的数据是**只读**的，不支持直接编辑。所有数据来自后端 API（如 `/api/v1/postback-info`、`/api/v1/pm-postback` 等）。

---

## 2. 系统入口与界面概览

### 2.1 进入方式

1. 鼠标点击左侧主导航栏 **Logs & Debug**（📋 图标）
2. 系统默认进入 **「Postback Log」** Tab

### 2.2 界面布局

Logs & Debug 包含 **2 个一级页面**，通过左侧二级导航切换：

| 页面 | 路由 | 图标 | 标题 | 副标题 | 功能状态 |
|------|------|------|------|--------|---------|
| `Logs` | `/logs/postback` | 📋 | Logs | Postback 全链路追踪 · 参数自动拆解 · 状态高亮 | ✅ 已上线 |
| `Debug` | `/logs/debug` | 🐛 | Debug | Error tracking, API debugging, and query management | ✅ 已上线 |

---

## 3. Logs 页面（Postback 全链路追踪）

Logs 页面包含 **4 个 Tab**，分别对应不同的日志维度：

| Tab Key | 标题 | API 端点 | 说明 |
|---------|------|-----------|------|
| `postback` | Postback Log | `/api/v1/postback-info` | Install 类型 Postback 日志（默认 Tab） |
| `event_postback` | Event Postback Log | `/api/v1/event-postback-info` | Event 类型 Postback 日志 |
| `pm_postback` | PM Postback Log | `/api/v1/pm-postback` | PM 维度 Postback 日志 |
| `pm_event` | PM Event Postback Log | `/api/v1/pm-event-postback` | PM 维度 Event Postback 日志 |

---

### 3.1 Postback Log Tab（Install 类型）

#### 3.1.1 筛选器

Postback Log Tab 包含 **8 个筛选字段**：

| 筛选字段 | 系统字段名 | 组件类型 | 说明 |
|----------|------------|---------|------|
| Date Range | `start_date` / `end_date` | DateRangePickerInline | 日期范围（支持快捷预设）|
| Package Name | `package_name` | Input | 应用包名关键词搜索 |
| Campaign | `campaign_ids` | Input（逗号分隔）| Campaign ID 多选 |
| T Campaign ID | `t_campaign_id` | Input | 第三方 Campaign ID 精确匹配 |
| Publisher | `publisher_ids` | MultiSelect | 渠道商多选 |
| Advertiser | `advertiser_ids` | MultiSelect | 广告主多选 |
| Country | `countries` | MultiSelect | 国家多选 |
| Channel | `channel` | Input | 渠道精确匹配 |

> 💡 **技巧**：Package Name 输入框支持按 `Enter` 键触发搜索，无需点击 Search 按钮。

#### 日期快捷预设（9 个）：

| 预设 | 说明 |
|------|------|
| 今天 | 当天 |
| 昨天 | 前一天 |
| 最近三天 | 前天 + 昨天 + 今天 |
| 最近七天 | 过去 7 天（含今天）|
| 最近三十天 | 过去 30 天（含今天）|
| 本周 | 本周一至今 |
| 上周 | 上周一至上周日 |
| 本月 | 本月 1 日至今 |
| 上月 | 上月整月 |

#### 3.1.2 表格字段（10 列）

| 列分组 | 列名 | 系统字段名 | 说明 | 特殊功能 |
|---------|------|------------|------|------------|
| **P0 - 时间** | Date | `date` | Postback 发送时间 | 显示日期 + 时间 |
| **P0 - 状态** | Status | `is_pb` / `abnormal` | Postback 状态 + 异常标记 | 已回调/未回调 + 异常次数 |
| **P0 - 投放** | Campaign | `t_campaign_id` / `campaign_id` | 第三方 Campaign ID（拼接格式：`id(name)`）| 可点击展开抽屉 |
| **P0 - 投放** | Pub / Adv | `publisher` / `advertiser` | 渠道商 + 广告主（拼接格式：`name(id)`）| — |
| **P0 - 财务** | Revenue / Payout | `revenue` / `payout` | 营收 + 成本 | 金额格式化 |
| **P1 - 事件** | Event | `event_name` | 事件名称 | 紫色标签 |
| **P1 - 地理** | Geo | `geo` | 国家 + 城市 | — |
| **P1 - 延迟** | Delay (s) | `delay` | Postback 延迟（秒）| 四档颜色规则（见下文）|
| **P2 - 错误** | Not PB Msg | `not_pb_msg` | Postback 失败原因 | 结构化展示（见下文）|

#### 3.1.3 Delay（延迟）四档颜色规则

| 延迟范围 | 颜色 | 说明 | 业务含义 |
|---------|------|------|------------|
| `0 < delay < 3s` | 🔴 红色加粗 | 疑似机器刷 | 延迟极短，可能是模拟器或脚本 |
| `3s ≤ delay < 3600s` | ✅ 绿色 | 正常区间 | 真实用户行为 |
| `3600s ≤ delay < 86400s` | 🟡 琥珀色加粗 | 延迟上报，需关注 | 用户延迟上报，需检查 PB 状态 |
| `delay ≥ 86400s` | 🔴 红色加粗 | 超长归因，建议核对 | 归因窗口外，可能导致财务对账争议 |

#### 3.1.4 Not PB Msg 结构化展示

当 Postback 失败时，`not_pb_msg` 字段会包含失败原因。系统会自动解析该字段，并结构化展示：

| 解析字段 | 说明 |
|----------|------|
| **HTTP Status** | HTTP 状态码（如 200、502、timeout）|
| **Response Time** | 响应耗时（ms）|
| **PB Range** | Postback 范围 |
| **Get** | 获取数量 |
| **Data** | 响应数据（尝试解析为 JSON）|
| **Reason** | 原因描述 |
| **分类标记** | Post-Attribution / CTIT Limit / Fraud Detected |

> 💡 **技巧**：点击 Not PB Msg 列中的 JSON 标签，可以展开查看完整 JSON 数据。

#### 3.1.5 行展开抽屉（Drawer）

点击表格中任意一行，系统从右侧滑出**抽屉组件**，展示该条 Postback 的完整信息。

抽屉包含 **3 个子 Tab**：

| 子 Tab | 说明 |
|--------|------|
| **概览** | 投放核心（Campaign/Advertiser/Publisher/Package）+ 财务（Revenue/Payout）|
| **归因链路** | 时序信息（Click Time / Conversion Time / Conversion Delay）+ 设备 ID |
| **安全反欺诈** | IP Trace（click_x_ips）+ Device & Location |

##### 概览子 Tab 字段：

| 分组 | 字段 | 说明 |
|------|------|------|
| 投放核心 | Campaign | Campaign ID + Campaign Name |
| 投放核心 | Advertiser | Advertiser Name + Advertiser ID |
| 投放核心 | Publisher | Publisher Name + Publisher ID |
| 投放核心 | Package Name | 应用包名 |
| 财务 | Revenue | 营收（绿色）|
| 财务 | Payout | 成本（蓝色）|
| 财务 | Geo | 国家 + 城市 |
| 财务 | Event | 事件名称 |
| 财务 | PB Server IP | Postback 服务器 IP |

##### 归因链路子 Tab 字段：

| 分组 | 字段 | 说明 |
|------|------|------|
| 时序信息 | Click Time | 点击时间 |
| 时序信息 | Conversion Time | 转化时间 |
| 时序信息 | Conversion Delay | 转化延迟（秒，四档颜色规则）|
| 设备 ID | Device ID (GAID/IDFA) | 设备 ID（可复制）|
| 设备 ID | Device Info | 设备信息（JSON 解析为 Brand/Model/OS/OS Version）|

##### 安全反欺诈子 Tab 字段：

| 分组 | 字段 | 说明 |
|------|------|------|
| IP Trace | click_x_ips | 点击 IP 列表（多 IP 风险预警：≥3 触发）|
| Device & Location | Device | 设备信息 |
| Device & Location | Location | 地理位置（Geo + Province + City）|

> ⚠️ **多 IP 风险预警**：当 `click_x_ips` 包含 3 个及以上 IP 时，系统会标记该条记录为**多 IP 风险**，并在抽屉中高亮显示。

#### 3.1.6 页内异常汇总统计（Sticky Summary）

表格上方会显示**当前页的异常汇总统计**：

| 统计项 | 说明 |
|---------|------|
| Total | 当前页总记录数 |
| Success | 已回调数量 |
| Failed | 未回调数量 |
| Abnormal | 异常数量（abnormal > 0）|

成功率百分比会根据阈值显示不同颜色：
- **≥ 95%**：绿色（正常）
- **80% - 95%**：琥珀色（需关注）
- **< 80%**：红色（异常）

#### 3.1.7 导出（Export）

导出功能用于将当前筛选条件下的 Postback 日志下载到本地。

##### 操作步骤：

1. 在筛选器中设置筛选条件
2. 点击 **Search** 按钮，加载数据
3. 点击表格右上角的 **Download** 按钮（⬇️ 图标）
4. 系统根据当前筛选条件，请求 `/api/v1/postback-info/export` 接口
5. 系统生成 CSV 文件并自动下载

> ⚠️ **注意**：导出的是**当前筛选条件**下的数据，不是整个表格的数据。

---

### 3.2 Event Postback Log Tab（Event 类型）

Event Postback Log Tab 与 Postback Log Tab **几乎一致**，仅有以下区别：

| 区别项 | Postback Log | Event Postback Log |
|--------|----------------|---------------------|
| **模式** | Install 模式（mode='install'） | Event 模式（mode='event'） |
| **默认日期范围** | 空（用户手动选）| 最近 3 天 |
| **额外列** | 无 | Event Value（事件价值）|
| **额外筛选字段** | 无 | Event Name（事件名称搜索）|

#### 操作步骤：

1. 切换到 **「Event Postback Log」** Tab
2. 系统自动填充日期范围为**最近 3 天**
3. （可选）在 **Event Name** 输入框中输入事件名称关键词
4. 点击 **Search** 按钮
5. 表格展示 Event 类型的 Postback 日志，比 Install 类型多一列 **Event Value**

---

### 3.3 PM Postback Log Tab（PM 维度）

PM Postback Log Tab 用于查看 **PM（Partner Manager）维度**的 Postback 日志。

#### 3.3.1 筛选器

PM Postback Log Tab 包含 **4 个筛选字段**：

| 筛选字段 | 系统字段名 | 组件类型 | 说明 |
|----------|------------|---------|------|
| Campaign ID | `campaign_id` | Input | AF Campaign ID 精确匹配 |
| Package Name | `package_name` | Input | App ID / Bundle ID 精确匹配 |
| Channel | `channel` | Input | AF Site ID 精确匹配 |
| Country | `country` | Input | Country Code 精确匹配 |

> ⚠️ **注意**：PM Postback Log Tab **不支持** Date Range 筛选（代码中 DateRangePickerInline 的 onChange 是空函数）。

#### 3.3.2 表格字段（10 列）

| 列名 | 系统字段名 | 说明 |
|------|------------|------|
| AF Campaign ID | `af_c_id` | AF Campaign ID |
| App ID / Bundle | `app_id` / `bundle_id` | App ID + Bundle ID |
| Channel | `af_siteid` | AF Site ID + Platform（iOS/Android）|
| Country | `country_code` | 国家代码 |
| Match Type | `match_type` | 匹配类型（deterministic / probabilistic）|
| Touch Type | `attributed_touch_type` | 归因触摸类型 |
| Click ID Match | `our_clickid_match` | Click ID 是否匹配（Yes/No）|
| Our Click ID | `our_click_id` | 平台 Click ID |
| Attributed Time | `attributed_touch_time` | 归因时间 |
| Info | `info` | 附加信息（JSON 对象，展示前 6 个 key）|
| PB Info | `pb_info` | Postback 信息 |

#### 3.3.3 特殊标记

| 标记 | 说明 |
|------|------|
| **Retargeting** | 当 `is_retargeting === 1` 时，显示紫色「Retargeting」标签 |
| **Match Type** | deterministic = 绿色，probabilistic = 琥珀色 |

---

### 3.4 PM Event Postback Log Tab（PM Event 维度）

PM Event Postback Log Tab 与 PM Postback Log Tab **几乎一致**，仅多一列：

| 区别项 | PM Postback Log | PM Event Postback Log |
|--------|-----------------|---------------------------|
| **额外列** | 无 | Event Name（事件名称，排第一列）|

#### 操作步骤：

1. 切换到 **「PM Event Postback Log」** Tab
2. 在筛选器中输入 **Campaign ID** / **Package Name** / **Channel** / **Country**
3. 点击 **Search** 按钮
4. 表格展示 PM Event 维度的 Postback 日志，比 PM Postback Log 多一列 **Event Name**

---

## 4. Debug 页面（错误追踪与 API 调试）

Debug 页面包含 **3 个 Tab**，分别对应不同的调试场景：

| Tab Key | 标题 | 说明 |
|---------|------|------|
| `error` | Error Report | 错误报告（聚合视图）|
| `debug` | Debug API | 调试 API（构造请求）|
| `query` | Query Task | 查询任务管理 |

---

### 4.1 Error Report Tab（错误报告）

Error Report Tab 用于**聚合查看错误分布**，帮助快速定位高频错误。

#### 4.1.1 筛选器

Error Report Tab 包含 **6 个筛选字段**：

| 筛选字段 | 系统字段名 | 组件类型 | 说明 |
|----------|------------|---------|------|
| Date Range | `start_date` / `end_date` | DateRangePickerInline | 日期范围（支持快捷预设）|
| Campaign ID | `campaign_id` | Input | Campaign ID 搜索 |
| Reason | `reason` | Select（单选）| 错误原因（如 CTIT_EXCEEDED、INVALID_PARAM 等）|
| Advertiser | `advertiser` | MultiSelect | 广告主多选 |
| Publisher | `publisher` | MultiSelect | 渠道商多选 |
| Country | `country` | MultiSelect | 国家多选 |

#### 4.1.2 Group By 选择器

Error Report Tab 支持按维度**聚合错误数据**：

| Group By 选项 | 说明 |
|----------------|------|
| **Reason** | 按错误原因聚合（默认）|
| **Advertiser** | 按广告主聚合 |
| **Publisher** | 按渠道商聚合 |
| **Country** | 按国家聚合 |
| **Campaign** | 按 Campaign 聚合 |

#### 4.1.3 聚合表格

表格展示聚合后的错误数据，包含 2 列：

| 列名 | 说明 |
|------|------|
| **Group By 维度** | 如 Reason、Advertiser 等 |
| **Clicks** | 该维度下的点击量（或错误量）|

> 💡 **技巧**：点击表格中的行，可以下钻查看该维度下的详细错误日志（功能待确认）。

---

### 4.2 Debug API Tab（调试 API）

Debug API Tab 用于**构造调试请求**，验证 API 参数和响应。

#### 4.2.1 请求配置

| 配置项 | 说明 |
|---------|------|
| **API Template** | API 模板 URL（如 `http://cpiapi.melodong.com/get_campaigns?publisher_id={publisher_id}&campaign_id={campaign_id}&access_token=...&debug=1`）|
| **Publisher Id** | 渠道商 ID（替换模板中的 `{publisher_id}`）|
| **Campaign Id** | Campaign ID（替换模板中的 `{campaign_id}`）|
| **Preview URL** | 替换后的完整 URL（实时预览）|

#### 4.2.2 操作步骤：

1. 在 **「Publisher Id」** 输入框中输入渠道商 ID
2. 在 **「Campaign Id」** 输入框中输入 Campaign ID
3. 系统实时生成 **Preview URL**（显示在 API Template 下方）
4. 点击 **「Send Request」** 按钮
5. 系统发送请求，并在**响应面板**中展示结果

#### 4.2.3 响应面板

响应面板展示 API 的响应内容，支持以下功能：

| 功能 | 说明 |
|------|------|
| **Copy** | 复制响应内容到剪贴板 |
| **JSON 格式化** | 响应内容自动格式化为 JSON（如果返回的是 JSON）|
| **语法高亮** | 代码语法高亮（深色背景）|

---

### 4.3 Query Task Tab（查询任务管理）

Query Task Tab 用于**管理长时间运行的查询任务**。

#### 4.3.1 任务表格

表格展示所有查询任务，包含 6 列：

| 列名 | 说明 |
|------|------|
| **Task Name** | 任务名称（如 `Error_Report_0331`）|
| **Create Time** | 创建时间（如 `14:31:55`）|
| **Filters (Summary)** | 筛选条件摘要（如 `Campaign: 6902530...`）|
| **Status** | 任务状态（Running / Success / Failed）|
| **Progress** | 任务进度（百分比或进度条）|
| **Action** | 操作按钮（见下文）|

#### 4.3.2 任务状态与操作

| 状态 | 说明 | 可用操作 |
|------|------|------------|
| **Running** | 任务运行中 | Cancel（取消）|
| **Success** | 任务成功 | Download（下载结果）+ Delete（删除）|
| **Failed** | 任务失败 | Retry（重试）+ Log（查看日志）|

#### 4.3.3 操作步骤：

1. 在 **「Search tasks...」** 输入框中输入关键词，筛选任务
2. 点击任务行的 **Action** 按钮，执行对应操作
3. 对于成功任务，点击 **Download** 按钮下载查询结果

---

## 5. 常见问题（FAQ）

### Q1：为什么我设置了筛选条件，表格数据没有变化？

**A**：Postback Log Tab 的筛选器是**延迟生效**的，需要点击 **Search** 按钮才会触发查询。请检查是否忘记点击 Search。

---

### Q2：为什么 PM Postback Log Tab 的 Date Range 选不了？

**A**：PM Postback Log Tab **不支持** Date Range 筛选（代码中 DateRangePickerInline 的 onChange 是空函数）。请使用其他筛选字段（如 Campaign ID、Package Name 等）。

---

### Q3：为什么 Event Postback Log Tab 的默认日期范围是最近 3 天？

**A**：Event 类型的 Postback 数据量通常较大，默认展示最近 3 天的数据可以避免加载过慢。你可以手动修改日期范围。

---

### Q4：为什么 Error Report Tab 的聚合表格只显示 5 个 Group？

**A**：当前系统是 Mock 数据，只展示了 5 个错误原因（CTIT_EXCEEDED、INVALID_PARAM、RATE_LIMIT、SIGNATURE_ERROR、SERVER_ERROR）。实际对接 API 后，会展示完整的聚合数据。

---

### Q5：如何快速复制某条 Postback 的 Click ID？

**A**：在 Postback Log 表格中，点击 **Click ID** 列中的 ID，系统会自动复制到剪贴板。或者在展开抽屉后，点击 **Click ID** 字段右侧的复制按钮。

---

## 附录：快捷操作索引

| 操作 | 入口 | 快捷键/说明 |
|------|------|-------------|
| 快速切换 Tab | Logs 页面顶部 | 点击 Tab 标签 |
| 快速选择日期预设 | Date Range 选择器 | 点击快捷预设按钮（如"最近七天"）|
| 快速触发搜索 | Package Name 输入框 | 按 `Enter` 键 |
| 快速复制 Click ID | Postback Log 表格 | 点击 Click ID 列中的 ID |
| 快速展开抽屉 | Postback Log 表格 | 点击任意一行 |
| 快速下载 JSON 日志 | 抽屉 Footer | 点击「下载 JSON 日志」按钮 |
| 快速导出 Postback 日志 | Postback Log 表格右上角 | 点击「Download」按钮 |
| 快速发送调试请求 | Debug API Tab | 输入 Publisher Id + Campaign Id，点击「Send Request」|
| 快速查看任务日志 | Query Task Tab | 点击 Failed 任务的「Log」按钮 |

---

*文档基于 `Logs.jsx`（2935 行）和 `Debug.jsx`（913 行）的真实代码编写，如有功能迭代请以系统实际界面为准。*

*最后更新：2026-06-22*
