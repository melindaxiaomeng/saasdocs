# 运营与用户操作手册
## 模块二：Campaign 创建与日常管理（Campaign Center SOP）

> **适用系统模块**：`Workplace → Campaign Center`  
> **目标读者**：商务（BD）、渠道经理（PM）、运营人员  
> **文档版本**：v1.1 · 2026-06-18

---

## 1. 业务场景与心智

Campaign（广告活动）是平台的**核心业务载体**——所有流量分发、转化回传、财务结算都围绕 Campaign 展开。

| 场景 | 说明 |
|------|------|
| 新广告主接入 | 广告主入驻后，为其创建对应 Campaign，配置 Package、Country、Weget（广告主单价）|
| 设定渠道成本 | 为每个 Campaign 配置 Payout（开给渠道的单价），控制平台利润空间 |
| 日常运维 | 调整 Cap、Weget、Payout、Block/Unblock，保障投放效率与利润 |
| 推单管理 | 复制推单信息发送给 Publisher，完成商务对接 |

### 核心业务字段说明

| 字段 | 含义 | 说明 |
|------|------|------|
| **Weget** | 广告主单价 | 平台**从广告主那里获取**的 Campaign 单价（收入）|
| **Payout** | 渠道单价 | 平台**开给渠道**的单价（成本）|
| **Margin** | 利润空间 | `Weget - Payout`，即每条转化的平台毛利 |

> ⚠️ **关键**：Weget 和 Payout 是两条独立的价格体系，调整时需同时关注，防止利润倒挂（Payout > Weget）。

---

## 2. 系统入口与界面概览

### 2.1 进入 Campaign Center

1. 鼠标点击左侧主导航栏 **Workplace**
2. 在展开的二级菜单中点击 **Campaign Center**

> 💡 **快捷键**：按 `Ctrl+K`（Windows）或 `Cmd+K`（Mac），输入「Campaign」快速跳转。

### 2.2 界面布局

```
┌─────────────────────────────────────────────────────┐
│  Keywords: [输入框]     Conditions: [筛选下拉]      │  ← 筛选区
│  Selected: [Active] [Blocked] [×××]              │
├─────────────────────────────────────────────────────┤
│  [+ New Campaign]  [Block] [More Actions ▼]      │  ← 操作栏
├─────────────────────────────────────────────────────┤
│  ID | Name | Package | Platform | Cap | Country  │  ← 数据表格
│  Weget | Clicks | Conv. | Memo | Update Time   │
├─────────────────────────────────────────────────────┤
│  < 1 2 3 ... 10 >                               │  ← 分页
└─────────────────────────────────────────────────────┘
```

---

## 3. Campaign 列表字段说明

表格各列含义如下（按分组展示）：

### 基础信息

| 列名 | 系统字段 | 说明 |
|------|----------|------|
| ID | `id` | Campaign 唯一标识，系统自动生成 |
| Name | `name` | Campaign 名称，便于识别 |
| Package Name | `package_name` | 应用包名（Android）或 Bundle ID（iOS）|
| Platform | `platform` | 操作系统：`Android` / `iOS` / `Both` |

### 定向

| 列名 | 系统字段 | 说明 |
|------|----------|------|
| Cap | `cap` | 日转化上限，超过后自动暂停 |
| Click Cap | `click_cap` | 日点击上限 |
| Steps | `steps` | 转化步骤数（如 Install → Purchase）|
| Country | `country` | 投放国家，支持多值 |
| Deviceidset Key | `deviceidset_key` | 设备 ID 去重标识 |

### 广告主

| 列名 | 系统字段 | 说明 | 是否可排序 |
|------|----------|------|-----------|
| Advertiser | `advertiser` | 所属广告主名称 | — |
| Weget | `weget` | 广告主单价（平台收入）| ✅（排序时映射为 `weget`）|
| AM | `am` | 负责 Account Manager | — |

### 数据

| 列名 | 系统字段 | 说明 | 是否可排序 |
|------|----------|------|-----------|
| Clicks | `clicks` | 累计点击量 | ✅ |
| Conv. | `conversions` | 累计转化量 | ✅ |

### 其他

| 列名 | 系统字段 | 说明 |
|------|----------|------|
| Memo | `memo` | 备注信息 |
| Update Time | `update_time` | 最后更新时间 |

> ⚠️ **注意**：`Clicks` 和 `Conversions` 为统计字段，数据来自后端聚合，可能有 5-10 分钟延迟。

---

## 4. 创建 Campaign（核心操作）

### 4.1 触发创建

有两种方式：

- **方式一**：在 Campaign Center 页面，点击右上角 **「+ New Campaign」** 按钮
- **方式二**：按 `Ctrl+K`，搜索「New Campaign」

系统打开创建抽屉（Drawer）。

> ⚠️ **待确认**：创建抽屉中的具体分组模块和字段，需要实际打开系统界面后补充。以下字段清单来自导出功能中的字段定义（`EXPORT_GROUPS`），创建表单的字段可能与此一致或部分一致。

### 4.2 字段清单（基于导出字段定义）

以下是系统中 Campaign 支持的所有字段，创建时可按实际需要填写：

#### ① Basic Info（基础信息）

| 字段 | 系统字段名 | 说明 |
|------|-----------|------|
| Campaign ID | `id` | 系统自动生成，创建时无需填写 |
| Name | `name` | Campaign 名称，建议命名规则：`广告主_国家_平台_日期` |
| Alias Name | `alias_name` | 别名，用于内部识别 |
| App Name | `app_name` | 应用名称 |
| Package Name | `package_name` | 应用包名（Android）或 Bundle ID（iOS）|
| Advertiser ID | `advertiser_id` | 所属广告主 ID，下拉选择 |
| T Campaign ID | `third_part_campaign_id` | 广告主侧的 Campaign ID（第三方对接用）|
| Status | `status` | 状态：`Active` / `Pending` / `Stop` |
| Label List | `label_list` | 标签，用于分类管理 |
| Memo | `memo` | 备注信息 |

#### ② Targeting（定向）

| 字段 | 系统字段名 | 说明 |
|------|-----------|------|
| Country | `country` | 投放国家，多选 |
| Platform | `platform` | 操作系统：`Android` / `iPhone` / `Web` / `All` |
| OS Version | `os_version` | 操作系统版本限制 |
| App Version | `app_version` | 应用版本限制 |
| Province | `province` | 省份定向（部分国家支持）|
| City | `city` | 城市定向 |
| Geo Target Must | `geo_target_must` | 是否强制 Geo 定向 |
| Device ID Must | `deviceid_must` | 是否强制 Device ID 校验 |

#### ③ Payout & KPI（单价与 KPI）

| 字段 | 系统字段名 | 说明 |
|------|-----------|------|
| Payout Type | `payout_type` | 计费类型（如 `CPA` / `CPC` / `CPM`）|
| **Weget** | `weget` | **广告主单价**（平台从广告主获取的单价）|
| KPI | `kpi` | KPI 目标值 |
| Event | `event` | 转化事件类型（如 `install` / `purchase`）|
| Cap | `cap` | 日转化上限 |
| Click Cap | `click_cap` | 日点击上限 |
| CVR Limit | `cvr_limit` | 转化率上限（风控用）|

#### ④ Links（追踪链接）

| 字段 | 系统字段名 | 说明 |
|------|-----------|------|
| Click URL | `click_url` | 平台生成的点击追踪链接 |
| Impression URL | `impression_url` | 曝光监测链接 |
| Preview URL | `preview_url` | 预览链接 |
| Redirect Steps | `redirect_steps` | 跳转步骤数 |
| Redirect Router | `redirect_router` | 跳转路由配置 |
| Is Store | `is_store` | 是否跳转到应用商店 |
| Is API | `is_api` | 是否 API 模式 |

#### ⑤ Risk Control（风控）

| 字段 | 系统字段名 | 说明 |
|------|-----------|------|
| Limit CTIT | `limit_ctit` | 限制点击-转化时间间隔 |
| Limit Conv. Rate | `limit_conversion_rate` | 转化率上限 |
| Limit Conv. Speed | `limit_conversion_speed` | 转化速度上限 |
| Block Channel | `block_channel` | 屏蔽渠道 |
| White List | `white_list` | 白名单 |
| Gross Clicks | `gross_clicks` | 总点击量（含作弊）|
| Gross Conversions | `gross_conversions` | 总转化量（含作弊）|

#### ⑥ Time（时间）

| 字段 | 系统字段名 | 说明 |
|------|-----------|------|
| Start Time | `start_time` | Campaign 开始时间 |
| End Time | `end_time` | Campaign 结束时间 |
| Update Time | `update_time` | 最后更新时间 |

### 4.3 提交创建

1. 检查表单无误后，点击抽屉底部的 **「Submit」** 按钮
2. 系统调用创建接口
3. 创建成功后：
   - 抽屉自动关闭
   - 列表刷新，新 Campaign 出现在第一行
   - 状态默认为 `Active`

---

## 5. 日常运维操作

### 5.1 查看 Campaign 详情

1. 在列表中找到目标 Campaign
2. 点击该行任意位置（除操作按钮外）
3. 系统打开详情抽屉，展示各分组信息

### 5.2 快速编辑（列表内联编辑）

部分字段支持在列表内直接点击编辑：

| 字段 | 系统字段名 | 操作方式 |
|------|-----------|---------|
| Weget | `weget` | 点击单元格，输入数值，回车保存 |
| Cap | `cap` | 点击单元格，输入数值，回车保存 |
| Click Cap | `click_cap` | 点击单元格，输入数值，回车保存 |

> 💡 内联编辑后系统自动调用更新接口，无需打开详情抽屉。

### 5.3 状态切换（Active ↔ Blocked）

**单个操作**：

1. 找到目标 Campaign
2. 点击该行右侧的状态操作按钮
3. 系统弹出确认框
4. 点击 **「Confirm」** 完成状态切换

**批量操作**：

1. 勾选列表中多个 Campaign（支持 `Shift` 连选、`Ctrl/Cmd` 多选）
2. 点击操作栏的 **「Block」** 按钮
3. 系统对所选 Campaign 批量执行状态切换

### 5.4 复制 Campaign ID / 复制推单信息

**复制 Campaign ID**：

1. 找到目标 Campaign
2. 点击该行右侧的复制按钮
3. 系统将 `id` 写入剪贴板

> ⚠️ **待确认**：推单信息复制功能是否已实现，格式如何？需要实际查看系统界面后补充。

### 5.5 导出 Campaign 数据

1. 在 Campaign Center 页面，点击操作栏的 **「Export」** 按钮
2. 系统打开导出字段选择弹窗
3. 按分组勾选需要导出的字段（支持搜索过滤）
4. 点击 **「Export」**，系统生成 CSV/Excel 文件并自动下载

> 💡 导出字段与本文档第 4.2 节字段清单一致，支持按需选择。

---

## 6. 筛选与查询

### 6.1 Keywords 搜索

支持按以下字段模糊搜索：

- Campaign ID（`campaign_ids`，精确搜索，支持逗号分隔多个）
- T Campaign ID（`third_part_campaign_id`，模糊搜索，支持逗号分隔多个）
- Campaign Name（`name`，模糊搜索）
- Package Name（`package_names`，精确搜索，支持逗号分隔多个）

**操作**：在筛选区 `Keywords` 输入框中键入关键词，系统实时过滤列表。

### 6.2 Conditions 筛选

点击 `Conditions` 下拉，展开高级筛选面板：

| 筛选维度 | 系统字段 | 说明 | 是否支持多选 |
|---------|---------|------|-------------|
| Advertiser | `advertiser_ids` | 按广告主筛选 | ✅ 多选 |
| Status | `statuses` | `Active`(1) / `Pending`(2) / `Stop`(3) | ✅ 多选 |
| Label | `label_list` | 按标签筛选 | ✅ 多选（支持 include/exclude 模式切换）|
| Device | `platforms` | `Android`(1) / `iPhone`(2) / `Web`(3) / `All`(4) | ✅ 多选 |
| Country | `country` | 按投放国家筛选 | ✅ 多选 |
| AM | `am` | 按 Account Manager 筛选 | ✅ 多选 |
| PID | `pids` | 按 Publisher ID 筛选 | 单选 |

### 6.3 快捷筛选标签

筛选区下方有多个快捷标签，点击可快速切换：

| 标签 | 说明 |
|------|------|
| `All` | 显示所有 Campaign |
| `Active` | 仅显示状态为 Active 的 Campaign |
| `Blocked` | 仅显示状态为 Blocked 的 Campaign |

---

## 7. 常见问题（FAQ）

### Q1：Weget 和 Payout 有什么区别？

**A**：
- **Weget** = 平台**从广告主获取**的单价（收入），如 Weget=5.0，表示广告主为每个转化支付平台 $5.0
- **Payout** = 平台**开给渠道**的单价（成本），如 Payout=4.0，表示平台为每个转化支付渠道 $4.0
- **Margin** = Weget - Payout = $1.0，即平台每条转化的毛利

### Q2：Campaign 创建后为何没有数据？

**A**：新 Campaign 需要 Publisher 侧开始投放后才有数据。请检查：
1. Click URL 是否已正确分发给 Publisher
2. 是否有真实流量进入（查看 Clicks 列是否有数值）
3. Weget 和 Payout 是否已正确配置

### Q3：为何 Campaign 自动暂停了？

**A**：可能是触发了 Cap 限额。查看该 Campaign 的 `Cap` 和 `Click Cap` 设置，如当日转化/点击量已达到上限，系统会自动将状态改为 `Stop`。

### Q4：创建 Campaign 时必须填写哪些字段？

**A**：至少需要填写 `Name`、`Advertiser ID`、`Package Name`、`Platform`、`Country`、`Weget`。其他字段可在创建后补充。

---

## 附录：快捷操作索引

| 操作 | 入口 | 说明 |
|------|------|------|
| 快速搜索 Campaign | 任意页面按 `Ctrl+K` | 输入「Campaign」|
| 新建 Campaign | Campaign Center → 「+ New Campaign」| — |
| 内联编辑 Weget/Cap | 点击列表单元格 | 输入后回车保存 |
| 批量 Block Campaign | 列表勾选 → 操作栏「Block」| 支持 Shift/Ctrl 多选 |
| 导出 Campaign 数据 | 操作栏「Export」| 支持字段自选 |
| 查看 Campaign 详情 | 点击列表行任意位置 | 打开详情抽屉 |
