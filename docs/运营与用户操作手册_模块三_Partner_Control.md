# 运营与用户操作手册
## 模块三：Partner Control（渠道-Publisher 绑定与流量控制 SOP）

> **适用系统模块**：`Workplace → Partner Control`  
> **目标读者**：商务（BD）、渠道经理（PM）、运营人员  
> **文档版本**：v1.0 · 2026-06-22

---

## 1. 业务场景与心智

Partner Control 是平台**渠道-Publisher 绑定关系的核心管理中心**，所有流量分发策略、单价配置、风控规则都在此模块配置。

| 场景 | 说明 |
|------|------|
| Advertiser 视角 | 选中一个广告主，管理其下所有 Campaign-Publisher 绑定关系 |
| Publisher 视角 | 选中一个渠道商，管理其下所有 Advertiser-Publisher 绑定关系 |
| 批量配置 | 通过 Publisher Global Rules 批量设置 Margin/Cap/风控规则 |
| 流量控制 | 通过 Campaign Routing 精确控制每个 Campaign 的 Publisher 分配 |
| 分组管理 | 通过 Group List 管理 Publisher 所属分组 |

> ⚠️ **关键概念**：Partner Control 有两种进入方式：
> 1. 主动进入：左侧导航 `Workplace → Partner Control`
> 2. 从 Partner Hub 跳转：点击 Advertiser/Publisher 行的「Control」按钮，系统自动选中对应实体并打开子 Tab

---

## 2. 系统入口与界面概览

### 2.1 进入 Partner Control

**方式一：主动进入**
1. 鼠标点击左侧主导航栏 **Workplace**
2. 在展开的二级菜单中点击 **Partner Control**

**方式二：从 Partner Hub 跳转**
1. 进入 `Partner Hub → Advertiser List`（或 `Publisher List`）
2. 找到目标实体，点击该行右侧的 **「Control」** 按钮
3. 系统自动打开 Partner Control 页面，并选中对应实体

> 💡 **快捷键**：按 `Ctrl+K`（Windows）或 `Cmd+K`（Mac），输入「Partner Control」快速跳转。

### 2.2 URL 参数说明

Partner Control 支持通过 URL 参数直接定位到指定实体：

| 参数 | 说明 | 示例 |
|------|------|---------|
| `tab` | 模式切换：`advertiser` 或 `publisher` | `?tab=advertiser` |
| `advertiserId` | 预选 Advertiser ID | `?tab=advertiser&advertiserId=1001` |
| `publisherId` | 预选 Publisher ID | `?tab=publisher&publisherId=2001` |

> 💡 从 Partner Hub 跳转时，系统会自动带上这些参数，无需手动拼接。

### 2.3 界面布局

```
┌─────────────────────────────────────────────────────┐
│  Partner Control                              │  ← 页面标题
├─────────────────────────────────────────────────────┤
│  [Advertiser] [Publisher]     [Adv: All ▼]  │  ← 模式切换 + 筛选器
├─────────────────────────────────────────────────────┤
│  Advertiser: AdvName (1001)                    │  ← Advertiser 模式：实体信息栏
│  [Campaign Routing] [Publisher Global Rules]     │  ← 二级 Tab
├─────────────────────────────────────────────────────┤
│  Campaign ID: [输入框]  Publisher: [下拉]     │  ← 筛选栏
│  [Import ▼] [Export]                          │  ← 操作栏
├─────────────────────────────────────────────────────┤
│  Publisher       Campaign      Payout   Margin  │  ← 数据表格
│  PubA (2001)    CampA (3001)   $1.50    30%  │
│  PubB (2002)    CampA (3001)   $1.20    25%  │
│  ...                                             │
├─────────────────────────────────────────────────────┤
│  < 1 2 3 ... 10 >                               │  ← 分页
└─────────────────────────────────────────────────────┘
```

---

## 3. Advertiser 模式

### 3.1 选择 Advertiser

1. 在 Partner Control 页面，确保顶部 Tab 选中 **「Advertiser」**
2. 在右侧下拉选择器（默认显示 `Adv: All`）中选择目标 Advertiser
3. 系统显示该 Advertiser 的信息栏，并默认进入 **Campaign Routing** 二级 Tab

> ⚠️ **注意**：选择器为单选模式（`single={true}`），只能同时选中一个 Advertiser。

### 3.2 二级 Tab 说明

Advertiser 模式下有两个二级 Tab：

| Tab | 说明 |
|-----|------|
| **Campaign Routing** | 管理该 Advertiser 下所有 Campaign-Publisher 绑定关系 |
| **Publisher Global Rules** | 管理该 Advertiser 的全局规则（批量配置 Margin/Cap/风控） |

---

## 4. Campaign Routing（Advertiser 模式）

### 4.1 业务场景与心智

Campaign Routing 是** Advertiser 视角下的绑定关系管理**，用于精确控制：
- 某个 Advertiser 的哪些 Campaign 可以分给哪些 Publisher
- 每个绑定关系的 Payout（开给渠道的单价）、Margin（利润率）、Cap（上限）
- Postback Rate（回传成功率阈值）、Hour Allow（允许投放时段）

### 4.2 表格字段说明

| 列名 | 系统字段 | 说明 | 是否可编辑 |
|------|----------|------|-------------|
| Publisher | `publisher_name` / `publisher_id` | 渠道商名称和 ID | — |
| Campaign | `campaign_name` / `campaign_id` | Campaign 名称和 ID | — |
| Payout | `payout` | 开给该 Publisher 的单价（成本）| ✅ 点击编辑 |
| Margin | `margin` | 利润率（`(Weget - Payout) / Weget * 100%`）| ✅ 点击编辑（输入百分比，如 `30` 代表 30%）|
| Cap | `cap` | 该绑定关系的日转化上限 | ✅ 点击编辑 |
| PostBack Rate | `postback_rate` | Postback 成功率阈值（%）| ✅ 点击编辑 |
| Hour Allow | `hours_allow` | 允许投放的时段（如 `9-23` 代表 9 点到 23 点）| ✅ 点击编辑 |
| Status | `status` | `Active`（活跃）/ `Paused`（暂停）| ✅ 点击切换 |
| Action | — | 删除该绑定关系 | ✅ 点击删除 |

> 💡 **内联编辑操作**：点击可编辑单元格，输入新值后按 `Enter` 保存，或点击其他区域自动保存。按 `Escape` 取消编辑。

### 4.3 状态切换（Active ↔ Paused）

**单个操作**：
1. 找到目标行
2. 点击 **Status** 单元格
3. 系统弹出确认框
4. 点击 **「Confirm」** 完成状态切换

> 💡 Status 单元格会显示不同颜色：
> - `Active`：绿色（`text-emerald-600 bg-emerald-50`）
> - `Paused`：黄色（`text-amber-600 bg-amber-50`）

### 4.4 删除绑定关系

1. 找到目标行
2. 点击 **Action** 列的 **「Remove」** 按钮（红色）
3. 系统立即删除该绑定关系（无确认框）

> ⚠️ **注意**：删除操作不可撤销，请谨慎操作。

### 4.5 筛选与查询

| 筛选器 | 说明 |
|---------|------|
| Campaign ID | 精确搜索，支持逗号分隔多个 ID |
| Publisher | 下拉多选，按 Publisher 过滤 |

### 4.6 批量导入（Import）

用于批量创建/更新 Campaign-Publisher 绑定关系。

#### 步骤 1：下载模板

1. 点击操作栏的 **「Import」** 下拉按钮
2. 选择 **「Download Template」**
3. 系统自动下载 `campaign_routing_template.csv` 文件

**模板字段说明**：

| 列名 | 系统字段 | 说明 | 示例 |
|------|----------|------|---------|
| Publisher ID | `publisher_id` | 渠道商 ID | `2001` |
| Campaign ID | `campaign_id` | Campaign ID | `3001` |
| Payout | `payout` | 开给渠道的单价 | `1.50` |
| Margin(%) | `margin` | 利润率（%），输入 `30` 代表 30% | `30` |
| Cap | `cap` | 日转化上限 | `1000` |
| Postback Rate(%) | `postback_rate` | Postback 成功率阈值（%）| `90` |
| Hours Allow | `hours_allow` | 允许投放时段 | `0-23` |

#### 步骤 2：填写模板

1. 打开下载的 CSV 模板
2. 按字段说明填写数据
3. 保存文件（支持 `.csv` / `.xlsx` / `.xls` 格式）

> 💡 **提示**：
> - 至少有一列有值（`publisher_id` 或 `campaign_id`）
> - 如果同时存在 `publisher_id` 和 `campaign_id`，系统会创建/更新精确绑定关系
> - 如果只有 `publisher_id`，系统会为该 Publisher 创建该 Advertiser 下所有 Campaign 的绑定关系（批量操作）

#### 步骤 3：上传文件

1. 点击操作栏的 **「Import」** 下拉按钮
2. 选择 **「Upload File」**
3. 在文件选择器中选中填写好的文件
4. 系统自动解析文件，并弹出 **Import Preview** 弹窗

#### 步骤 4：预览并确认

1. 在 Import Preview 弹窗中，检查解析出的数据（最多显示所有行）
2. 确认无误后，点击右下角的 **「Confirm (N)」** 按钮（N 为解析出的记录数）
3. 系统调用 `batchCreatePublisherCampaignRelations` 接口，批量创建/更新绑定关系
4. 导入完成后，表格自动刷新

> ⚠️ **注意**：
> - 如果导入失败，系统会提示失败数量。请检查模板格式是否正确。
> - 导入操作是**增量更新**，不会删除已有绑定关系。

### 4.7 导出（Export）

1. 点击操作栏的 **「Export」** 按钮
2. 系统生成当前筛选条件下的所有数据，并自动下载 CSV 文件

---

## 5. Publisher Global Rules（Advertiser 模式）

### 5.1 业务场景与心智

Publisher Global Rules 是** Advertiser 视角下的全局规则配置**，用于批量管理该 Advertiser 下所有 Publisher 的：
- **Delivery & Scaling**（流量控制）：控制点击上限、自动绑定 Campaign 等
- **Environment & Targeting**（环境定向）：控制 OS、Country、Package Name、White Channels 等
- **Safety & Quality**（安全质量）：控制 Re Cap、Postback Rate、Filter Steps、Min/Max Weget 等

> 💡 **核心价值**：一次配置，应用到该 Advertiser 下所有 Publisher。无需逐个 Publisher 配置，极大提升效率。

### 5.2 表格字段说明

| 列名 | 系统字段 | 说明 | 是否可编辑 |
|------|----------|------|-------------|
| Publisher | `publisher_name` / `publisher_id` | 渠道商名称和 ID | — |
| Margin | `margin` | 全局利润率（%），显示格式 `30%` | ✅ 点击编辑 |
| Cap | `cap` | 全局日转化上限 | ✅ 点击编辑 |
| Click Cap | `campaign_click_cap` | 全局日点击上限 | ✅ 点击编辑（展开行内）|
| Postback Rate | `postback_rate` | Postback 成功率阈值（%）| ✅ 点击编辑（展开行内）|
| Re Cap | `reject_cap` | 拒绝上限（风控用）| ✅ 点击编辑（展开行内）|
| Update Time | `update_time` | 最后更新时间 | — |
| Remarks | `remarks` | 备注信息 | ✅ 点击编辑 |
| Operate | — | 删除该 Publisher 的全局规则 | ✅ 点击删除 |

### 5.3 展开行详情

点击每行左侧的 **展开按钮**（▶），可以展开该 Publisher 的详细配置，分为三个分组：

#### ① Delivery & Scaling（流量控制）

| 字段 | 系统字段 | 编辑类型 | 说明 |
|------|----------|---------|------|
| Hourly Click Cap | `hourly_click_cap` | input | 每小时点击上限 |
| Per Cam Click Cap | `per_campaign_click_cap` | input | 每个 Campaign 的点击上限 |
| Per Cam Cap | `per_campaign_cap` | input | 每个 Campaign 的转化上限 |
| Auto Bind Campaign | `auto_bind_campaign` | switch | 是否自动绑定新创建的 Campaign |

#### ② Environment & Targeting（环境定向）

| 字段 | 系统字段 | 编辑类型 | 说明 |
|------|----------|---------|------|
| OS | `platform` | iconSelect | 操作系统：`AND`（Android）/ `iOS`（iOS）/ `WEB`（Web）|
| Country | `country` | multiSelect | 投放国家，支持逗号分隔多个 |
| Package Name List | `package_name_list` | expandable | 应用包名列表，逗号分隔 |
| White Channels | `white_channels` | input | 白名单渠道 |
| White Channels Type | `white_channels_type` | switch | 白名单匹配模式：`Enabled`（精确匹配）/ `Disabled`（模糊匹配）|

#### ③ Safety & Quality（安全质量）

| 字段 | 系统字段 | 编辑类型 | 说明 |
|------|----------|---------|------|
| Filter Steps | `filter_steps` | input | 过滤步数（风控用）|
| Min Weget | `min_weget` | input | 最小 Weget（广告主单价）阈值，$ 格式 |
| Max Weget | `max_weget` | input | 最大 Weget（广告主单价）阈值，$ 格式 |

### 5.4 内联编辑操作

**普通字段（input 类型）**：
1. 点击可编辑单元格
2. 输入新值
3. 点击其他区域或按 `Enter` 保存
4. 按 `Escape` 取消编辑

**Switch 类型（Auto Bind Campaign / White Channels Type）**：
1. 点击 switch 按钮
2. 系统自动切换状态并保存

**Icon Select 类型（OS）**：
1. 点击对应图标（`AND` / `iOS` / `WEB`）
2. 系统自动切换选中状态并保存

**Multi Select 类型（Country）**：
1. 点击单元格，输入逗号分隔的国家代码
2. 点击其他区域自动保存

**Expandable 类型（Package Name List）**：
1. 点击单元格，输入逗号分隔的包名
2. 非编辑状态下，只显示前 3 个包名，超出部分显示 `+N`

### 5.5 展开/收起所有行

- 点击表头左侧的 **展开按钮**（▶），可以展开/收起所有行
- 展开状态下，按钮变为 ▼

### 5.6 新增 Publisher 全局规则

1. 点击右上角的 **「New」** 按钮
2. 系统弹出 **选择 Publisher** 弹窗
3. 在搜索框中输入 Publisher 名称或 ID 进行过滤
4. 在列表中选择目标 Publisher（单选）
5. 点击 **「确认添加」** 按钮
6. 系统调用 `createPublisherAdvertiserRelation` 接口，创建绑定关系
7. 创建成功后，表格自动刷新

> ⚠️ **注意**：
> - 如果所选 Publisher 已存在，系统会弹出警示框，提示「Publisher 已存在」，请勿重复添加。
> - 新增时，系统会使用默认规则（Margin=0%、Cap=0、各开关为 Disabled）。

### 5.7 删除 Publisher 全局规则

**单个删除**：
1. 找到目标行
2. 点击 **Operate** 列的 **「×」** 按钮
3. 系统弹出确认框（`window.confirm`）
4. 点击 **「确定」** 完成删除

**批量删除**：
1. 勾选表格左侧的复选框，选择要删除的行（支持全选）
2. 点击表格上方的 **「批量删除」** 按钮
3. 系统弹出确认框
4. 点击 **「确定」** 完成批量删除

> ⚠️ **注意**：删除操作不可撤销，请谨慎操作。

---

## 6. Publisher 模式

### 6.1 选择 Publisher

1. 在 Partner Control 页面，确保顶部 Tab 选中 **「Publisher」**
2. 在右侧下拉选择器（默认显示 `Pub: All`）中选择目标 Publisher
3. 系统显示该 Publisher 的信息栏，并默认进入 **Advertiser List** 子 Tab

> ⚠️ **注意**：选择器为单选模式（`single={true}`），只能同时选中一个 Publisher。

### 6.2 子 Tab 说明

Publisher 模式下有五个子 Tab：

| Tab | 说明 |
|-----|------|
| **Advertiser List** | 管理该 Publisher 下所有 Advertiser-Publisher 绑定关系 |
| **Group List** | 管理该 Publisher 所属的 Group（分组）|
| **Campaign List** | 管理该 Publisher 下所有 Campaign-Publisher 绑定关系 |
| **Block Campaign** | 管理该 Publisher 被 Block 的 Campaign 列表 |
| **Publisher Global Rules** | 管理该 Publisher 的全局规则（同 Advertiser 模式的 Publisher Global Rules）|

---

## 7. Advertiser List（Publisher 模式）

### 7.1 业务场景与心智

Advertiser List 是** Publisher 视角下的绑定关系管理**，功能与 Advertiser 模式的 Publisher Global Rules 类似，但聚焦于：
- 该 Publisher 与哪些 Advertiser 有绑定关系
- 每个绑定关系的 Margin、Cap、风控规则

### 7.2 表格字段说明

| 列名 | 系统字段 | 说明 | 是否可编辑 |
|------|----------|------|-------------|
| Advertiser | `advertiser_name` / `advertiser_id` | 广告主名称和 ID | — |
| Margin | `margin` | 利润率（%），显示格式 `30%` | ✅ 点击编辑 |
| Cap | `cap` | 日转化上限 | ✅ 点击编辑 |
| Click Cap | `campaign_click_cap` | 日点击上限 | ✅ 点击编辑（展开行内）|
| Postback Rate | `postback_rate` | Postback 成功率阈值（%）| ✅ 点击编辑（展开行内）|
| Re Cap | `reject_cap` | 拒绝上限（风控用）| ✅ 点击编辑（展开行内）|
| Update Time | `update_time` | 最后更新时间 | — |
| Remarks | `remarks` | 备注信息 | ✅ 点击编辑 |
| Operate | — | 删除该 Advertiser 的绑定关系 | ✅ 点击删除 |

### 7.3 展开行详情

同 Advertiser 模式的 Publisher Global Rules（见第 5.3 节）。

### 7.4 新增 Advertiser 绑定关系

1. 点击右上角的 **「New」** 按钮
2. 系统弹出 **选择 Advertiser** 弹窗
3. 在搜索框中输入 Advertiser 名称或 ID 进行过滤
4. 在列表中选择目标 Advertiser（单选）
5. 点击 **「确认添加」** 按钮
6. 系统调用 `createPublisherAdvertiserRelation` 接口，创建绑定关系
7. 创建成功后，表格自动刷新

> ⚠️ **注意**：如果所选 Advertiser 已存在，系统会弹出警示框，提示「Advertiser 已存在」。

---

## 8. Group List（Publisher 模式）

### 8.1 业务场景与心智

Group List 是** Publisher 视角下的分组管理**，用于：
- 管理该 Publisher 所属的 Group（分组）
- 每个 Group 可以配置不同的 Margin Rate、Cap、Postback Rate
- 通过分组，可以批量管理不同业务线的 Publisher

### 8.2 表格字段说明

| 列名 | 系统字段 | 说明 | 是否可编辑 |
|------|----------|------|-------------|
| Publisher ID | `publisher_id` | 渠道商 ID（只读）| — |
| Group ID | `group_id` | 分组 ID | ✅ 点击编辑（新增时填写）|
| Margin Rate | `margin_rate` | 利润率（%），显示格式 `30%` | ✅ 点击编辑 |
| Cap | `cap` | 日转化上限 | ✅ 点击编辑 |
| Postback Rate | `postback_rate` | Postback 成功率阈值（%）| ✅ 点击编辑 |
| Status | `status` | `Active`（活跃）/ `Inactive`（停用）| ✅ 点击切换 |
| Operate | — | 删除该 Group | ✅ 点击删除 |

### 8.3 状态切换（Active ↔ Inactive）

1. 找到目标行
2. 点击 **Status** 列的 switch 按钮
3. 系统自动切换状态并保存

### 8.4 删除 Group

1. 找到目标行
2. 点击 **Operate** 列的 **「×」** 按钮
3. 系统立即删除该 Group（无确认框）

> ⚠️ **注意**：删除操作不可撤销，请谨慎操作。

### 8.5 新增 Group

1. 点击右上角的 **「New」** 按钮
2. 系统弹出 **新增 Group** 弹窗
3. 填写表单：
   - **Publisher ID**（只读）：当前 Publisher 的 ID
   - **Group ID***（必填）：输入 Group ID
   - **Margin Rate (%)***（必填）：输入利润率（%），如 `30` 代表 30%
   - **Cap (金额)***（必填）：输入日转化上限，如 `5000`
   - **Postback Rate (%)***（必填）：输入 Postback 成功率阈值（%），如 `95`
4. 点击 **「确认」** 按钮
5. 系统将新 Group 添加到表格中（前端模拟，实际应调用 API）

> ⚠️ **注意**：Group ID 不能重复，否则系统会提示「Group ID 已存在」。

---

## 9. Campaign List（Publisher 模式）

### 9.1 业务场景与心智

Campaign List 是** Publisher 视角下的绑定关系管理**，功能与 Advertiser 模式的 Campaign Routing 类似，但聚焦于：
- 该 Publisher 与哪些 Campaign 有绑定关系
- 每个绑定关系的 Weget、Payout、Margin、Cap、Postback Rate、Hour Allow
- 支持展开行查看详细配置（流量策略、渠道与平台、风控与权重）

### 9.2 表格字段说明

| 列名 | 系统字段 | 说明 | 是否可编辑 |
|------|----------|------|-------------|
| Campaign | `campaign_name` / `campaign_id` | Campaign 名称和 ID | — |
| Publisher | `publisher_name` / `publisher_id` | 渠道商名称和 ID | — |
| Weget | `weget` | 广告主单价（平台收入）| ✅ 点击编辑 |
| Payout | `payout` | 开给渠道的单价（成本）| ✅ 点击编辑 |
| Margin | `margin` | 利润率（%），显示格式 `30%` | ✅ 点击编辑 |
| Cap | `cap` | 日转化上限 | ✅ 点击编辑 |
| Postback Rate | `postback_rate` | Postback 成功率阈值（%）| ✅ 点击编辑（展开行内）|
| Hour Allow | `hours_allow` | 允许投放的时段 | ✅ 点击编辑（展开行内）|
| Status | `status` | `Active`（活跃）/ `Paused`（暂停）| ✅ 点击切换 |
| Action | — | 删除该绑定关系 | ✅ 点击删除 |

### 9.3 展开行详情

点击每行左侧的 **展开按钮**（▶），可以展开该 Campaign 的详细配置，分为三个分组：

#### ① 流量策略（Traffic Policy）

| 字段 | 系统字段 | 编辑类型 | 说明 |
|------|----------|---------|------|
| Click Cap | `click_cap` | input | 日点击上限 |
| Hourly Click Cap | `hourly_click_cap` | input | 每小时点击上限 |
| Per Cam Click Cap | `per_campaign_click_cap` | input | 每个 Campaign 的点击上限 |
| Per Cam Cap | `per_campaign_cap` | input | 每个 Campaign 的转化上限 |
| Auto Bind Campaign | `auto_bind_campaign` | switch | 是否自动绑定新创建的 Campaign |

#### ② 渠道与平台（Channel & Platform）

| 字段 | 系统字段 | 编辑类型 | 说明 |
|------|----------|---------|------|
| OS | `platform` | iconSelect | 操作系统：`AND`（Android）/ `iOS`（iOS）/ `WEB`（Web）|
| Country | `country` | multiSelect | 投放国家，支持逗号分隔多个 |
| Package Name List | `package_name_list` | expandable | 应用包名列表，逗号分隔 |
| White Channels | `white_channels` | input | 白名单渠道 |
| White Channels Type | `white_channels_type` | switch | 白名单匹配模式：`Enabled`（精确匹配）/ `Disabled`（模糊匹配）|

#### ③ 风控与权重（Risk & Weight）

| 字段 | 系统字段 | 编辑类型 | 说明 |
|------|----------|---------|------|
| Re Cap | `reject_cap` | input | 拒绝上限（风控用）|
| Reject Rate | `reject_rate` | input | 拒绝率阈值（%）|
| Filter Steps | `filter_steps` | input | 过滤步数（风控用）|
| Min Weget | `min_weget` | input | 最小 Weget（广告主单价）阈值，$ 格式 |
| Max Weget | `max_weget` | input | 最大 Weget（广告主单价）阈值，$ 格式 |

### 9.4 内联编辑操作

同 Advertiser 模式的 Campaign Routing（见第 4.2 节）。

### 9.5 状态切换（Active ↔ Paused）

同 Advertiser 模式的 Campaign Routing（见第 4.3 节）。

### 9.6 删除绑定关系

同 Advertiser 模式的 Campaign Routing（见第 4.4 节）。

### 9.7 筛选与查询

| 筛选器 | 说明 |
|---------|------|
| Advertiser ID | 精确搜索，支持逗号分隔多个 ID |
| Campaign ID | 精确搜索，支持逗号分隔多个 ID |
| Package Name | 精确搜索，支持逗号分隔多个包名 |
| Country | 下拉多选，按 Country 过滤 |
| Status | 下拉单选，按 Status 过滤 |

---

## 10. Block Campaign（Publisher 模式）

### 10.1 业务场景与心智

Block Campaign 是** Publisher 视角下的拦截管理**，用于：
- 管理该 Publisher 被 Block 的 Campaign 列表
- 可以 Block 某个 Campaign（不让它分给该 Publisher）
- 可以 Unblock 某个 Campaign（恢复分配）

### 10.2 表格字段说明

| 列名 | 系统字段 | 说明 | 是否可编辑 |
|------|----------|------|-------------|
| Campaign Name (ID) | `campaign_name` / `campaign_id` | Campaign 名称和 ID | — |
| Channels | `channels` | 渠道列表（白名单）| ✅ 点击编辑 |
| Desc | `desc` | 描述信息 | ✅ 点击编辑 |
| Status | `status` | `Active`（活跃）/ `Stop`（停用）| ✅ 点击切换 |

### 10.3 内联编辑操作

**Channels 字段**：
1. 点击 Cells 单元格
2. 输入渠道列表（逗号分隔）
3. 点击其他区域自动保存

**Desc 字段**：
1. 点击 Desc 单元格
2. 输入描述信息
3. 点击其他区域自动保存

### 10.4 状态切换（Active ↔ Stop）

1. 找到目标行
2. 点击 **Status** 列的 switch 按钮
3. 系统自动切换状态并保存

### 10.5 删除 Block Campaign

1. 找到目标行
2. 点击 **Operate** 列的 **「×」** 按钮
3. 系统弹出确认框（`confirm`）
4. 点击 **「确定」** 完成删除

> ⚠️ **注意**：删除操作不可撤销，请谨慎操作。

### 10.6 新增 Block Campaign

1. 点击右上角的 **「New」** 按钮
2. 系统弹出 **New Block Campaign** 弹窗
3. 填写表单：
   - **Campaign ID***（必填）：输入 Campaign ID
   - **Channels**：输入渠道列表（默认 `ALL`）
   - **Desc**：输入描述信息
   - **Status**：开启/关闭（默认 `Active`）
4. 点击 **「Confirm」** 按钮
5. 系统调用 `createPublisherCampaignBlock` 接口，创建 Block 记录
6. 创建成功后，表格自动刷新

> ⚠️ **注意**：如果所选 Campaign ID 已存在，系统会提示「Campaign ID 已存在」。

---

## 11. 常见问题（FAQ）

### Q1：Campaign Routing 和 Publisher Global Rules 有什么区别？

**A**：
- **Campaign Routing**：管理**精确到 Campaign-Publisher** 的绑定关系，每个关系可以有不同的 Payout、Margin、Cap。
- **Publisher Global Rules**：管理** Advertiser-Publisher** 的全局规则，一次配置应用到该 Advertiser 下所有 Campaign。

**使用建议**：
- 如果某个 Publisher 对所有 Campaign 的配置都相同，使用 **Publisher Global Rules** 批量配置。
- 如果某个 Publisher 对不同 Campaign 有不同的 Payout/Margin，使用 **Campaign Routing** 精确配置。

### Q2：为什么我编辑了某个字段，但表格没有刷新？

**A**：系统使用**无感刷新**机制，编辑后会自动调用 `refreshData()` 刷新表格。如果刷新失败，请检查：
1. 网络连接是否正常
2. 是否被后端权限限制（如只读账号）

### Q3：Import 模板中的 Margin(%) 应该填什么？

**A**：填百分比数值，如 `30` 代表 30%。系统存储时会转换为小数（`0.3`），显示时再转换为百分比（`30%`）。

### Q4：为什么我无法删除某个绑定关系？

**A**：请检查：
1. 是否有**后端权限**（部分账号可能只有只读权限）
2. 该绑定关系是否**已被其他模块引用**（如 Finance 财务报表）

### Q5：Group List 中的 Margin Rate 和 Campaign Routing 中的 Margin 有什么区别？

**A**：
- **Group List** 中的 Margin Rate 是**分组级别**的利润率，应用到该 Group 下所有 Campaign。
- **Campaign Routing** 中的 Margin 是**绑定关系级别**的利润率，只应用到该 Campaign-Publisher 绑定关系。

**优先级**：绑定关系级别 > 分组级别 > 全局级别。

---

## 附录：快捷操作索引

| 操作 | 入口 | 说明 |
|------|------|------|
| 快速进入 Partner Control | 任意页面按 `Ctrl+K` | 输入「Partner Control」|
| 从 Advertiser 跳转 | Partner Hub → Advertiser List → 「Control」| 自动选中该 Advertiser |
| 从 Publisher 跳转 | Partner Hub → Publisher List → 「Control」| 自动选中该 Publisher |
| 内联编辑字段 | 点击单元格 | 输入后回车保存 |
| 展开/收起所有行 | 点击表头左侧展开按钮 | 批量展开/收起 |
| 批量删除 | 勾选复选框 → 点击「批量删除」| 支持全选 |
| 批量导入 | 点击「Import」→「Upload File」| 支持 .csv/.xlsx/.xls |
| 导出数据 | 点击「Export」| 自动下载 CSV |
