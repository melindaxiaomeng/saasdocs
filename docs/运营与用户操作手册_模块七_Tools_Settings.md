# 运营与用户操作手册
## 模块七：Tools & Settings（工具与设置）

> **文档版本**：v1.0  
> **撰写日期**：2026-03-25  
> **目标读者**：内部运营人员、客户成功团队、使用 SaaS 平台的客户  
> **前置要求**：已完成模块一（Partner Hub）和模块二（Campaign Center）的配置

---

## 目录

1. [模块概述](#1-模块概述)
2. [Traffic Policy Center（流量策略中心）](#2-traffic-policy-center流量策略中心)
3. [Mapping Workspace（映射工作区）](#3-mapping-workspace映射工作区)
4. [Global Settings（全局设置）](#4-global-settings全局设置)
5. [常见问题 FAQ](#5-常见问题-faq)

---

## 1. 模块概述

**Tools & Settings** 是平台的配置工具中心，提供流量分配策略、数据映射管理和全局配置功能。

### 1.1 访问路径

| 子模块 | 导航路径 | 说明 |
|--------|----------|------|
| Traffic Policy Center | 左侧菜单 → **Tools & Settings** → **Traffic Policy Center** | 流量策略与渠道替换管理 |
| Mapping Workspace | 左侧菜单 → **Tools & Settings** → **Mapping Workspace** | PKG/Appname 与 Event Name 映射 |
| Global Settings | 左侧菜单 → **Tools & Settings** → **Global Settings** | 全局配置与预警规则 |

### 1.2 核心功能地图

```
Tools & Settings
├── Traffic Policy Center
│   ├── Traffic Policy（流量策略）
│   │   ├── 策略列表（筛选、快速编辑、批量操作）
│   │   ├── Policy Simulator（策略模拟器）
│   │   └── 新增/编辑策略
│   └── Replace Channel（渠道替换）
│       ├── 替换规则列表
│       └── 新增/编辑替换规则
├── Mapping Workspace
│   ├── PKG / Appname（包名与应用名映射）
│   │   ├── 映射列表（筛选、快速编辑）
│   │   └── 新增映射
│   └── Event Name（事件名映射）
│       ├── 映射列表（筛选、编辑）
│       └── 新增/编辑映射
└── Global Settings
    ├── Global Settings（全局配置）
    ├── Warning Rules（预警规则）
    └── Warning Logs（预警日志）
```

---

## 2. Traffic Policy Center（流量策略中心）

### 2.1 功能说明

Traffic Policy Center 用于管理流量分配策略和渠道替换规则，控制不同广告主、Campaign、渠道的流量走向。

**两个核心 Tab：**
- **Traffic Policy**：配置流量分配策略（按优先级 P0-P3 匹配）
- **Replace Channel**：配置渠道替换规则（将流量从原渠道替换到目标渠道）

### 2.2 Traffic Policy（流量策略）

#### 2.2.1 策略列表

**页面路径**：`Tools & Settings` → `Traffic Policy Center` → `Traffic Policy` Tab

**筛选条件**：

| 筛选字段 | 说明 | 交互方式 |
|----------|------|----------|
| Advertiser | 按广告主筛选 | 下拉多选（懒加载，支持 Select All / Clear） |
| Publisher | 按渠道筛选 | 下拉多选（懒加载，支持 Select All / Clear） |
| Campaign | 按 Campaign 筛选 | 下拉多选（懒加载，支持 Select All / Clear） |
| Status | 按状态筛选 | 下拉多选（Active / Inactive） |
| Search | 全局搜索 | 输入框（支持回车触发） |

**Active Filters**（已选筛选条件）：
- 显示当前所有筛选条件
- 每个筛选条件可单独移除（点击 ✕ 按钮）
- 支持 **Clear All** 一键清空

**表格字段**：

| 字段 | 说明 | 备注 |
|------|------|------|
| ID | 策略 ID | 自增唯一标识 |
| Advertiser | 广告主 | 显示 `name(id)` 格式 |
| Campaign | Campaign | 显示 `name(id)` 格式，若为 "All" 表示适用于所有 Campaign |
| Publisher | 渠道 | 显示 `name(id)` 格式，若为 "ALL" 表示适用于所有渠道 |
| Priority | 优先级 | P0 > P1 > P2 > P3（P0 优先级最高） |
| Trigger Type | 触发类型 | Click（点击）、Event（事件）、Random（随机） |
| Weight | 权重 | 流量分配权重（仅 Random 类型有效） |
| Status | 状态 | Active（启用）/ Inactive（停用） |
| Actions | 操作 | 编辑、删除 |

**优先级规则**：
```
P0: Advertiser + Campaign + Publisher（最具体，优先级最高）
P1: Advertiser + Campaign
P2: Campaign（All Advertisers & Publishers）
P3: Advertiser（All Campaigns）
Global: 全局策略（优先级最低）
```

**快速编辑（Quick Edit）**：
- 点击表格中的 **Weight** 或 **Status** 字段，直接进入编辑模式
- **Weight**：输入框，回车保存
- **Status**：开关切换（ToggleSwitch）
- 保存后自动更新，无需刷新页面

**批量操作**：
- 勾选多条策略（复选框）
- 点击 **Batch Active**（批量启用）或 **Batch Stop**（批量停用）
- 确认后批量更新状态

#### 2.2.2 Policy Simulator（策略模拟器）

**功能说明**：模拟流量匹配过程，验证策略配置是否正确。

**使用步骤**：
1. 在页面右侧找到 **Policy Simulator** 面板
2. 输入测试参数：
   - **Advertiser ID**：输入广告主 ID
   - **Campaign ID**：输入 Campaign ID
   - **Publisher ID**：输入渠道 ID
3. 点击 **Run Simulation** 按钮
4. 查看模拟结果：
   - **Matched Policy**：匹配到的策略（显示 ID、优先级、触发类型）
   - **Match Steps**：匹配过程（显示每条策略的匹配情况）
   - **Final Decision**：最终决策（使用哪个策略）

**模拟逻辑**：
- 按优先级排序（P0 > P1 > P2 > P3）
- 只检查状态为 **Active** 的策略
- 逐条检查是否匹配（Advertiser ID、Campaign ID、Publisher ID）
- 返回第一条匹配到的策略

#### 2.2.3 新增/编辑策略

**新增策略**：
1. 点击页面右上角 **+ New Policy** 按钮
2. 填写表单：

| 字段 | 说明 | 必填 |
|------|------|------|
| Advertiser | 广告主 | 是 |
| Campaign | Campaign | 否（留空表示适用于所有 Campaign） |
| Publisher | 渠道 | 否（留空表示适用于所有渠道） |
| Trigger Type | 触发类型 | 是（Click / Event / Random） |
| Weight | 权重 | 否（仅 Random 类型需要） |
| Status | 状态 | 是（Active / Inactive） |

3. 点击 **Save** 保存

**编辑策略**：
1. 点击表格中的 **Edit** 按钮（铅笔图标）
2. 修改表单字段
3. 点击 **Save** 保存

**删除策略**：
1. 点击表格中的 **Delete** 按钮（垃圾桶图标）
2. 确认删除

### 2.3 Replace Channel（渠道替换）

#### 2.3.1 替换规则列表

**页面路径**：`Tools & Settings` → `Traffic Policy Center` → `Replace Channel` Tab

**筛选条件**：
- Advertiser（下拉多选）
- Publisher（下拉多选）
- Campaign（下拉多选）
- Status（下拉多选）

**表格字段**：

| 字段 | 说明 | 备注 |
|------|------|------|
| ID | 替换规则 ID | 自增唯一标识 |
| Advertiser | 广告主 | 显示 `name(id)` 格式 |
| Campaign | Campaign | 显示 `name(id)` 格式 |
| Original Channel | 原渠道 | 流量来源渠道 |
| Replace Channel | 替换渠道 | 流量目标渠道 |
| Status | 状态 | Active（启用）/ Inactive（停用） |
| Actions | 操作 | 编辑、删除 |

#### 2.3.2 新增/编辑替换规则

**新增替换规则**：
1. 点击页面右上角 **+ New Replace** 按钮
2. 填写表单：

| 字段 | 说明 | 必填 |
|------|------|------|
| Advertiser | 广告主 | 是 |
| Campaign | Campaign | 是 |
| Original Channel | 原渠道 | 是 |
| Replace Channel | 替换渠道 | 是 |
| Status | 状态 | 是（Active / Inactive） |

3. 点击 **Save** 保存

**编辑替换规则**：
1. 点击表格中的 **Edit** 按钮
2. 修改表单字段
3. 点击 **Save** 保存

**删除替换规则**：
1. 点击表格中的 **Delete** 按钮
2. 确认删除

---

## 3. Mapping Workspace（映射工作区）

### 3.1 功能说明

Mapping Workspace 用于管理渠道包名、应用名和事件名的映射关系，确保数据在不同系统之间的正确传递。

**两个核心 Tab：**
- **PKG / Appname**：管理包名（Package Name）与应用名（App Name）的映射
- **Event Name**：管理渠道事件名与内部事件名的映射

### 3.2 PKG / Appname（包名与应用名映射）

#### 3.2.1 映射列表

**页面路径**：`Tools & Settings` → `Mapping Workspace` → `PKG / Appname` Tab

**筛选条件**：

| 筛选字段 | 说明 | 交互方式 |
|----------|------|----------|
| Publisher | 按渠道筛选 | 下拉多选（懒加载） |
| Advertiser | 按广告主筛选 | 下拉多选（懒加载） |
| Package Name | 按包名筛选 | 输入框 |
| Status | 按状态筛选 | 下拉多选（Active / Stop） |

**表格字段**：

| 字段 | 说明 | 备注 |
|------|------|------|
| Package Name | 包名 | 可快速编辑（点击进入编辑模式，回车保存） |
| App Name | 应用名 | 可快速编辑 |
| Platform | 平台 | iOS / Android（标签区分颜色） |
| Publisher | 渠道 | 显示 `name(id)` 格式 |
| Channel | 渠道名称 | 可快速编辑 |
| Advertiser | 广告主 | 显示 `name(id)` 格式 |
| Remarks | 备注 | 可快速编辑 |
| Status | 状态 | 开关切换（Active ↔ Stop） |
| Link | 链接参数 | 悬停显示生成的链接参数（`packagename=xxx&appname=xxx`） |

**快速编辑**：
- 点击 **Package Name**、**App Name**、**Channel**、**Remarks** 字段，进入编辑模式
- 编辑时显示 **✓**（保存）和 **✕**（取消）按钮
- 回车保存，ESC 取消

**状态切换**：
- 点击 **Status** 列的开关按钮，切换 Active ↔ Stop
- 切换后自动保存

**Link 参数预览**：
- 悬停在 **Link** 列的图标上，显示生成的链接参数
- 格式：`&packagename=xxx&appname=xxx`

#### 3.2.2 新增映射

**操作步骤**：
1. 点击页面右上角 **Add New** 按钮
2. 填写表单：

| 字段 | 说明 | 必填 |
|------|------|------|
| Advertiser | 广告主 | 是（下拉单选） |
| Publisher | 渠道 | 是（下拉单选） |
| Channel | 渠道名称 | 否 |
| Platform | 平台 | 是（iOS / Android，默认 iOS） |
| Package Name | 包名 | 是 |
| App Name | 应用名 | 是 |
| Remarks | 备注 | 否 |

3. 点击 **Save** 保存

**字段说明**：
- **Package Name**：应用的包名（如 `com.game.casual.frenzy`）
- **App Name**：应用名称（如 `Casual Frenzy Pro`）
- **Platform**：iOS 或 Android
- **Publisher**：渠道（如 AppLovin、Unity Ads）
- **Channel**：渠道下的具体渠道名称（如 AppLovin Display、Unity Rewarded）
- **Advertiser**：广告主（如 Nike、Adidas）

### 3.3 Event Name（事件名映射）

#### 3.3.1 映射列表

**页面路径**：`Tools & Settings` → `Mapping Workspace` → `Event Name` Tab

**筛选条件**：
- Campaign（输入框）
- Publisher（下拉多选）

**表格字段**：

| 字段 | 说明 | 备注 |
|------|------|------|
| Campaign | Campaign | 显示 `name(id)` 格式 |
| Publisher | 渠道 | 显示 `name(id)` 格式 |
| Mapping Event Name | 映射事件名 | 显示 `事件名 → 映射事件名` 的键值对列表 |
| Description | 描述 | 备注信息 |
| Status | 状态 | 开关切换（Active ↔ Stop） |
| Actions | 操作 | 编辑 |

**Mapping Event Name 显示格式**：
- 每条记录可以映射多个事件名
- 显示格式：`key → value`（如 `af_purchase → purchase`）
- 每个映射对占一行

#### 3.3.2 新增/编辑映射

**新增映射**：
1. 点击页面右上角 **Add New** 按钮
2. 填写表单：

| 字段 | 说明 | 必填 |
|------|------|------|
| Publisher | 渠道 | 是（下拉单选） |
| Campaign | Campaign | 是（输入框，填写 Campaign ID） |
| Mapping Info | 映射信息 | 是（键值对列表） |
| Description | 描述 | 否 |
| Status | 状态 | 是（Active / Stop） |

3. 点击 **+ Add** 添加映射对
4. 填写每个映射对的 **Event Name**（渠道事件名）和 **Mapping Event Name**（内部事件名）
5. 点击 **Save** 保存

**编辑映射**：
1. 点击表格中的 **Edit** 按钮
2. 修改表单字段
3. 可以添加/删除映射对
4. 点击 **Save** 保存

**删除映射**：
1. 编辑映射时，点击映射对右侧的 **✕** 按钮
2. 保存后生效

---

## 4. Global Settings（全局设置）

### 4.1 功能说明

Global Settings 用于管理平台的全局配置、预警规则和预警日志。

**三个核心 Tab：**
- **Global Settings**：全局配置项管理
- **Warning Rules**：预警规则配置
- **Warning Logs**：预警日志记录

### 4.2 Global Settings（全局配置）

#### 4.2.1 配置列表

**页面路径**：`Tools & Settings` → `Global Settings` → `Global Settings` Tab

**表格字段**：

| 字段 | 说明 | 备注 |
|------|------|------|
| Key | 配置键 | 配置项名称 |
| Value | 配置值 | 配置项值 |
| Description | 描述 | 配置项说明 |
| Status | 状态 | Active（启用）/ Inactive（停用） |
| Actions | 操作 | 编辑、删除 |

**新增配置**：
1. 点击页面右上角 **+ New Setting** 按钮
2. 填写表单：
   - **Key**：配置键
   - **Value**：配置值
   - **Description**：描述
   - **Status**：状态
3. 点击 **Save** 保存

**编辑配置**：
1. 点击表格中的 **Edit** 按钮
2. 修改表单字段
3. 点击 **Save** 保存

**删除配置**：
1. 点击表格中的 **Delete** 按钮
2. 确认删除

### 4.3 Warning Rules（预警规则）

#### 4.3.1 规则列表

**页面路径**：`Tools & Settings` → `Global Settings` → `Warning Rules` Tab

**表格字段**：

| 字段 | 说明 | 备注 |
|------|------|------|
| Rule Name | 规则名称 | 预警规则名称 |
| Type | 类型 | 预警类型（如 `conversion_rate`、`margin`、`cap_remaining`） |
| Threshold | 阈值 | 触发预警的阈值（如 `< 5%`、`> 90%`） |
| Target | 目标范围 | 规则作用的目标（如 `Campaign`、`Publisher`） |
| Level | 级别 | Critical（严重）、Warning（警告）、Info（提示） |
| Enabled | 启用 | 开关切换（启用 ↔ 禁用） |
| Actions | 操作 | 编辑、删除 |

**级别颜色**：
- **Critical**：红色标签（`bg-red-100 text-red-700`）
- **Warning**：黄色标签（`bg-yellow-100 text-yellow-700`）
- **Info**：蓝色标签（`bg-blue-100 text-blue-700`）

**新增规则**：
1. 点击页面右上角 **+ New Rule** 按钮
2. 填写表单：
   - **Rule Name**：规则名称
   - **Type**：类型
   - **Operator**：运算符（`<`、`>`、`=`）
   - **Threshold**：阈值
   - **Target**：目标范围
   - **Level**：级别
   - **Enabled**：是否启用
3. 点击 **Save** 保存

**编辑规则**：
1. 点击表格中的 **Edit** 按钮
2. 修改表单字段
3. 点击 **Save** 保存

**删除规则**：
1. 点击表格中的 **Delete** 按钮
2. 确认删除

**启用/禁用规则**：
- 点击 **Enabled** 列的开关按钮，切换启用 ↔ 禁用

### 4.4 Warning Logs（预警日志）

#### 4.4.1 日志列表

**页面路径**：`Tools & Settings` → `Global Settings` → `Warning Logs` Tab

**筛选条件**：
- Date Range（日期范围，使用 DateRangePickerInline 组件）
- Level（级别：Critical / Warning / Info）
- Status（状态：Pending / Handled）

**日期预设**：
- 今天、昨天、最近三天、最近七天、最近三十天
- 本周、上周、本月、上月

**表格字段**：

| 字段 | 说明 | 备注 |
|------|------|------|
| Time | 时间 | 预警触发时间 |
| Rule Name | 规则名称 | 触发预警的规则 |
| Level | 级别 | Critical / Warning / Info |
| Message | 消息 | 预警详情 |
| Target | 目标 | 预警作用的目标 |
| Status | 状态 | Pending（待处理）/ Handled（已处理） |
| Actions | 操作 | 查看详情、处理 |

**查看详情**：
1. 点击表格中的 **View** 按钮（眼睛图标）
2. 显示预警详情（弹窗）
3. 查看预警的完整信息

**处理预警**：
1. 点击表格中的 **Handle** 按钮（勾选图标）
2. 确认处理
3. 状态变更为 **Handled**

**批量处理**：
1. 勾选多条预警记录
2. 点击 **Batch Handle**（批量处理）
3. 确认处理

---

## 5. 常见问题 FAQ

### Q1: Traffic Policy 的优先级怎么理解？

**A**: 优先级按 P0 > P1 > P2 > P3 排序，P0 最具体，优先级最高。系统按优先级顺序匹配策略，匹配到第一条后立即停止。

**示例**：
- P0: Advertiser=1, Campaign=1, Publisher=1（最具体）
- P1: Advertiser=1, Campaign=1（较具体）
- P2: Campaign=1（较宽泛）
- P3: Advertiser=1（宽泛）

如果流量同时匹配 P0 和 P1，使用 P0 策略。

### Q2: Replace Channel 和 Traffic Policy 有什么区别？

**A**:
- **Traffic Policy**：控制流量分配策略（如按权重分配、按点击事件触发）
- **Replace Channel**：将原渠道的流量替换到目标渠道（如将流量从 Channel A 替换到 Channel B）

### Q3: Mapping Workspace 的 PKG/Appname 和 Event Name 有什么用？

**A**:
- **PKG/Appname**：将渠道的包名和应用名映射到内部名称，确保数据正确传递
- **Event Name**：将渠道的事件名映射到内部事件名（如将 `af_purchase` 映射为 `purchase`）

### Q4: Warning Rules 的阈值怎么设置？

**A**: 阈值格式为 `运算符 + 数值 + 单位`，如：
- `< 5%`：低于 5% 时触发
- `> 90%`：高于 90% 时触发
- `= 0`：等于 0 时触发

### Q5: 如何快速定位到某个预警日志？

**A**: 使用筛选条件：
1. 选择日期范围（使用日期预设或自定义范围）
2. 选择级别（Critical / Warning / Info）
3. 选择状态（Pending / Handled）
4. 点击 **Search** 按钮

---

## 附录：API 接口清单

| 功能 | 接口 | 方法 | 说明 |
|------|------|------|------|
| Traffic Policy | `/api/v1/traffic-policy/` | GET | 获取策略列表 |
| Traffic Policy | `/api/v1/traffic-policy/` | POST | 新增策略 |
| Traffic Policy | `/api/v1/traffic-policy/{id}/` | PUT | 更新策略 |
| Traffic Policy | `/api/v1/traffic-policy/{id}/` | DELETE | 删除策略 |
| Replace Channel | `/api/v1/replace-channel/` | GET | 获取替换规则列表 |
| Replace Channel | `/api/v1/replace-channel/` | POST | 新增替换规则 |
| Replace Channel | `/api/v1/replace-channel/{id}/` | PUT | 更新替换规则 |
| Replace Channel | `/api/v1/replace-channel/{id}/` | DELETE | 删除替换规则 |
| PKG/Appname | `/api/v1/advertiser-publisher-pkg-map/` | GET | 获取映射列表 |
| PKG/Appname | `/api/v1/advertiser-publisher-pkg-map/` | POST | 新增映射 |
| PKG/Appname | `/api/v1/advertiser-publisher-pkg-map/{id}/` | PUT | 更新映射 |
| Event Name | `/api/v1/mapping-publisher-event/` | GET | 获取事件映射列表 |
| Event Name | `/api/v1/mapping-publisher-event/` | POST | 新增事件映射 |
| Event Name | `/api/v1/mapping-publisher-event/{id}/` | PUT | 更新事件映射 |
| Global Settings | `/api/v1/global-settings/` | GET | 获取全局配置列表 |
| Warning Rules | `/api/v1/warning-rules/` | GET | 获取预警规则列表 |
| Warning Rules | `/api/v1/warning-rules/` | POST | 新增预警规则 |
| Warning Rules | `/api/v1/warning-rules/{id}/` | PUT | 更新预警规则 |
| Warning Logs | `/api/v1/warning-logs/` | GET | 获取预警日志列表 |

---

**文档结束**

> **下一步**：完成模块七后，建议继续阅读 **模块八：User Management（用户管理）**，了解如何管理用户、菜单权限和资源权限。
