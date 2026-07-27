# 运营与用户操作手册
## 模块四：Finance（财务对账与 Scrub 管理 SOP）

> **适用系统模块**：`Finance → Monthly Report`  
> **目标读者**：财务（Finance）、商务（AM）、渠道经理（PM）、运营人员  
> **文档版本**：v1.0 · 2026-06-22

---

## 1. 业务场景与心智

Finance 是平台**财务对账的核心模块**，所有营收（Revenue）、成本（Payout）、Scrub（清洗抵扣）、确认金额（Confirm Rev./Confirm Pay.）都在此模块管理。

| 场景 | 说明 |
|------|------|
| 月度对账 | 每月初，AM/PM 对上月数据进行确认（Confirm），作为财务结算依据 |
| Scrub 管理 | 广告主拒绝部分转化时，在系统里录入 Scrub Rev./Scrub/Cover，调整应收/应付金额 |
| 异常排查 | 通过 Discrepancy Rate、Reason 字段，快速定位广告主与平台数据不一致的记录 |
| 批量操作 | 通过 Batch Confirm、Batch Adjustment，高效处理大量待确认记录 |
| 对账单上传 | 上传广告主对账单（Statement），系统自动解析并比对，生成异常报告 |

> ⚠️ **核心概念**：
> - `Revenue` = 广告主单价 × 转化数（平台应收）
> - `Payout` = 渠道单价 × 转化数（平台应付）
> - `Scrub Revenue` = 广告主确认营收（Revenue - 清洗抵扣）
> - `Scrub` = 渠道侧清洗抵扣金额
> - `Cover` =  cover 金额（额外补偿/抵扣）
> - `C.Revenue` = `Revenue - Scrub Revenue`（确认应收）
> - `C.Payout` = `Payout - Scrub + Cover`（确认应付）

---

## 2. 系统入口与界面概览

### 2.1 进入 Finance

1. 鼠标点击左侧主导航栏 **Finance**
2. 系统默认进入 **Monthly Report** 子页面

> 💡 **快捷键**：按 `Ctrl+K`（Windows）或 `Cmd+K`（Mac），输入「Finance」快速跳转。

### 2.2 界面布局

```
┌─────────────────────────────────────────────────────┐
│  Finance                                      │  ← 页面标题
│  Finance / Monthly Report                       │
├─────────────────────────────────────────────────────┤
│  [Date Range] [Campaign ID] [Advertiser ▼]  │  ← 核心筛选器
│  [Publisher ▼] [Third Part ID] [Advanced]  │
│  [Upload] [Export] [Reset] [Search]         │
├─────────────────────────────────────────────────────┤
│  Found N abnormal records（红色警告条）        │  ← 异常提示
├─────────────────────────────────────────────────────┤
│  Pub.  Adv.  Camp.  Revenue  Scrub Rev.  │  ← 数据表格
│  ...                                          │
├─────────────────────────────────────────────────────┤
│  Total: Revenue / C.Revenue / Payout / ...   │  ← 合计行
├─────────────────────────────────────────────────────┤
│  < 1 2 3 ... 10 >                               │  ← 分页
└─────────────────────────────────────────────────────┘
```

---

## 3. 筛选与查询

### 3.1 核心筛选器

| 筛选器 | 说明 | 操作 |
|---------|------|-------|
| Date Range | 对账月份范围 | 点击选择起始/结束月份；支持快捷按钮：`This Month` / `Last Month` / `Q1` |
| Campaign ID | 按 Campaign ID 精确搜索 | 输入单个 ID，支持逗号分隔多个 |
| Advertiser | 按广告主过滤 | 下拉多选（懒加载，首次打开时请求 API） |
| Publisher | 按渠道商过滤 | 下拉多选（懒加载） |
| Third Part ID | 按第三方 Campaign ID 过滤 | 输入 `t_campaign_id` |

### 3.2 高级筛选器（Advanced Filters）

点击筛选栏第二行的 **「Advanced Filters ▶」** 按钮，展开高级筛选：

| 筛选器 | 说明 |
|---------|------|
| Package Name | 按应用包名过滤 |
| Country | 按投放国家过滤（下拉多选，懒加载） |
| OS | 按操作系统过滤（`iOS` / `Android` / `Web`）|
| AM Status | 按 AM 确认状态过滤（`pending` / `confirmed`）|
| PM Status | 按 PM 确认状态过滤（`pending` / `confirmed`）|

### 3.3 分组视图（Group By）

在筛选栏下方，点击分组标签切换视图：

| 标签 | 说明 |
|------|------|
| **Detail View (No Grouping)** | 明细视图，显示 Pub./Adv./Camp. 三列 |
| **By Advertiser** | 按广告主聚合，只显示 Adv. 列 |
| **By Publisher** | 按渠道商聚合，只显示 Pub. 列 |
| **By Campaign** | 按 Campaign 聚合，只显示 Camp. 列 |

> 💡 分组视图下，表格会自动显示合计行（Total Row），方便查看聚合数据。

### 3.4 差异率筛选（Discrepancy Rate Filter）

在代码中预留了差异率筛选功能（通过 `discRateFilter` 状态），可以筛选：
- `gt0` = 差异率 > 0 的记录（存在异常）
- `eq0` = 差异率 = 0 的记录（完全对齐）

> ⚠️ **待确认**：该筛选器在当前界面是否有对应的 UI 控件？还是需要手动修改代码触发？

---

## 4. 表格字段说明

### 4.1 维度列（动态显示）

| 列名 | 系统字段 | 说明 | 是否可编辑 |
|------|----------|------|-------------|
| Pub. | `publisher` / `publisher_id` | 渠道商名称和 ID，格式 `name(id)` | — |
| Adv. | `advertiser` / `advertiser_id` | 广告主名称和 ID，格式 `name(id)` | — |
| Camp. | `campaign` / `campaign_id` | Campaign 名称和 ID，格式 `name(id)` | — |

> 💡 分组视图下，只会显示选中的维度列。

### 4.2 营收概览列（Revenue Overview）

| 列名 | 系统字段 | 说明 | 是否可编辑 | 着色规则 |
|------|----------|------|-------------|-----------|
| Revenue | `revenue` | 广告主营收（原始）| — | — |
| Scrub Rev. | `scrubRevenue` | 广告主确认营收（清洗后）| ✅ 点击编辑 | 清洗率 ≤ 3% 绿色；≤ 10% 黄色；> 10% 红色加粗 |
| Confirm Rev. | `cRevenue` | 最终确认营收（`Revenue - Scrub Revenue`）| — | 同 Scrub Rev. 着色规则 |

> 💡 **Scrub Rev. 快速编辑**：
> - 点击单元格，输入绝对值或百分比
> - 右上角有 **⇄% / ⇄Val** 切换按钮，切换输入模式
> - 输入百分比时，系统自动计算绝对值并联动更新 Scrub、S Conv. 字段

### 4.3 成本概览列（Payout Overview）

| 列名 | 系统字段 | 说明 | 是否可编辑 | 着色规则 |
|------|----------|------|-------------|-----------|
| Payout | `payout` | 渠道成本（原始）| — | — |
| Scrub | `scrub` | 渠道清洗抵扣金额 | ✅ 点击编辑 | 清洗率 ≤ 3% 绿色；≤ 10% 黄色；> 10% 红色加粗 |
| Cover | `cover` | 补偿/抵扣金额 | ✅ 点击编辑 | — |
| Confirm Pay. | `cPayout` | 最终确认成本（`Payout - Scrub + Cover`）| — | 同 Scrub 着色规则 |

> 💡 **Scrub 快速编辑**：
> - 支持绝对值/百分比切换（同 Scrub Rev.）
> - 修改 Scrub 会自动重新计算 C.Payout

### 4.4 转化数据列（Conversion Data）

| 列名 | 系统字段 | 说明 | 是否可编辑 |
|------|----------|------|-------------|
| Conversions | `conversions` | 总转化数 | — |
| PB Conv. | `pbConv` | Postback 转化数（渠道上报）| ✅ 点击编辑 |
| S.Conv. | `sConv` | Scrub 转化数（清洗后）| ✅ 点击编辑 |

> 💡 **转化数着色**：
> - `Conversions < PB Conv.` → 红色（渠道多报）
> - `Conversions > PB Conv.` → 绿色（平台多记）
> - `Conversions = PB Conv.` → 灰色（正常）

### 4.5 Scrub Reason 列

| 列名 | 系统字段 | 说明 | 是否可编辑 |
|------|----------|------|-------------|
| Scrub Reason | `reason` | Scrub 原因（多个原因用 `;` 分隔）| ✅ 点击编辑 |

**Reason 分类（系统预定义）**：

| 分类 | 颜色 | 说明 |
|------|------|------|
| `Fraud` | 红色 | 发现欺诈流量，广告主拒绝结算相关转化 |
| `CTIT limit` | 橙色 | Click-to-Install 时间过短，触发异常阈值，相关转化被抵扣 |
| `Post-Attribution` | 紫色 | 后置归因导致广告主延迟确认转化，可能影响本期结算金额 |
| `CTIT` | 橙色 | CTIT 异常，相关转化被抵扣 |

> 💡 点击 Reason 单元格，可以查看详细的原因描述（通过 `ReasonModal` 弹窗）。

### 4.6 状态列（可选显示）

默认显示状态列。点击表格上方右侧的 **「📊 Hide Status Columns」** 可以隐藏状态列，扩大数据列的显示空间。

| 列名 | 系统字段 | 说明 | 操作 |
|------|----------|------|-------|
| AM Status | `aStatus` | AM（商务）确认状态 | `pending` → 点击 **「Confirm」** 按钮确认；`confirmed` → 点击标签取消确认 |
| PM Status | `pStatus` | PM（渠道经理）确认状态 | 同上 |

> 💡 **状态列显示信息**：
> - `confirmed` 状态：显示绿色标签 + 确认人 + 确认时间（鼠标悬停 `i` 图标查看）
> - `pending` 状态：显示 **「Confirm」** 按钮（虚线边框，点击确认）

---

## 5. 快速编辑操作

### 5.1 内联编辑（Inline Edit）

支持内联编辑的字段：`Scrub Rev.`、`Scrub`、`Cover`、`S.Conv.`、`PB Conv.`、`Reason`。

**操作步骤**：

1. 点击可编辑单元格
2. 输入新值
   - **绝对值模式**：直接输入金额（如 `1.50`）
   - **百分比模式**：输入百分比（如 `5` 代表 5%）
3. 按 `Enter` 保存，或点击其他区域自动保存
4. 按 `Escape` 取消编辑

> 💡 **Scrub Rev. 百分比模式下的联动逻辑**：
> 输入百分比后，系统会自动计算：
> - `Scrub Revenue` = `Revenue × 百分比`
> - `Scrub` = `Payout × 百分比`
> - `S.Conv.` = `Conversions × 百分比`
> 
> 实现"按比例一键 Scrub"。

### 5.2 绝对值/百分比模式切换

在 `Scrub Rev.`、`Scrub`、`S.Conv.` 列的标题右侧，有一个**淡入淡出的切换按钮**：

- 鼠标悬停列标题时，按钮出现
- 点击 **「⇄%」** 切换到百分比模式
- 点击 **「⇄Val」** 切换回绝对值模式

> 💡 切换模式后，单元格显示格式会相应变化：
> - 绝对值模式：显示 `1.50`
> - 百分比模式：显示 `5.0%`

---

## 6. 状态确认操作

### 6.1 AM Status（商务确认）

**单个确认**：

1. 找到目标行
2. 点击 **AM Status** 列的 **「Confirm」** 按钮
3. 系统调用 `updateMonthReportStatus` API，传入 `events: ['am_confirm']`
4. 状态变为 `confirmed`，标签显示为绿色

**单个取消确认**：

1. 找到已确认的行
2. 点击 **AM Status** 列的 **「Confirmed」** 标签
3. 系统调用 `updateMonthReportStatus` API，传入 `events: ['am_unconfirm']`
4. 状态恢复为 `pending`

### 6.2 PM Status（渠道经理确认）

操作同 AM Status，只是调用的 API 事件为 `['pm_confirm']` / `['pm_unconfirm']`。

### 6.3 批量确认（Batch Confirm）

1. 勾选表格左侧的复选框，选择要确认的行（支持全选）
2. 表格上方会出现**批量操作栏**（蓝色渐变背景）
3. 点击 **「✓ Batch Confirm AM Status」** 按钮，批量确认 AM 状态
4. 点击 **「✓ Batch Confirm PM Status」** 按钮，批量确认 PM 状态

> 💡 批量确认会同时调用多条 API，每条记录单独请求但并行发送。

---

## 7. 批量调整（Batch Adjustment）

用于批量修改多条记录的 `Scrub Rev.`、`Scrub`、`S.Conv.` 字段。

### 操作步骤

1. 勾选表格左侧的复选框，选择要调整的行
2. 点击批量操作栏的 **「− Batch Adjustment」** 按钮（橙色）
3. 系统弹出 **Batch Adjustment** 弹窗
4. 在弹窗中填写要调整的值：

| 字段 | 输入模式 | 说明 |
|------|----------|------|
| Scrub Revenue | `number`（绝对值）/ `percent`（百分比）| 调整广告主确认营收 |
| Scrub | `number`（绝对值）/ `percent`（百分比）| 调整渠道清洗抵扣金额 |
| S Conv. | `number`（绝对值）/ `percent`（百分比）| 调整 Scrub 转化数 |

5. 点击 **「Confirm」** 按钮
6. 系统批量更新选中记录，并自动重新计算 `C.Revenue` / `C.Payout`

> 💡 **百分比模式说明**：
> - 输入 `5` 代表 5%
> - 系统按每条记录的原值 × (1 - 百分比) 计算新值
> - 如原 `Scrub Revenue` = 100，输入 5%，新值 = 100 × (1 - 0.05) = 95

---

## 8. Upload Statement（上传对账单）

### 8.1 业务场景与心智

每月初，广告主会提供对账单（Statement），列出他们认为应结算的营收金额。通过 Upload Statement 功能：

1. **前端解析**：上传 Excel/CSV 文件，系统自动解析并映射到 Finance 表格格式
2. **差异比对**：自动计算 `Disc.%`（差异率），标记异常记录
3. **微型战报**：解析完成后，显示匹配数、完美对齐数、异常数
4. **确认更新**：点击 **「Confirm & Update」**，将解析结果批量提交到后端

### 8.2 操作步骤

#### 步骤 1：上传文件

1. 在 Finance 页面，点击筛选栏的 **「Upload Statement」** 按钮
2. 系统弹出 **Upload Statement** 弹窗（宽度 1400px）
3. 拖拽文件到上传区域，或点击 **「Select File」** 按钮选择文件
   - 支持格式：`.xlsx`、`.xls`、`.csv`
   - 文件大小限制：未明确，建议 < 10MB

#### 步骤 2：解析文件

1. 上传后，系统自动解析文件（显示 `Parsing...` 状态）
2. 解析完成后，显示 **Parse Successful** 状态和微型战报：

```
✓ Parse Successful
12 rows parsed    12 matched    0 anomalies
```

3. 如果解析失败，会显示错误信息（如"File is empty"）

#### 步骤 3：查看预览表格

解析成功后，弹窗下方会显示**预览表格**（Finance 表格样式），包含以下列：

| 列名 | 说明 |
|------|------|
| Pub. | 渠道商（`name(id)` 格式）|
| Adv. | 广告主（`name(id)` 格式）|
| Camp. | Campaign（`name(id)` 格式）|
| Revenue | 营收 |
| Scrub Rev. | 清洗后营收 |
| Payout | 成本 |
| Scrub | 清洗抵扣 |
| Cover | 补偿/抵扣 |
| Conv. | 转化数 |
| PB Conv. | Postback 转化数 |
| S.Conv. | Scrub 转化数 |
| C.Revenue | 确认营收 |
| Status | `OK`（绿色）/ `Anomaly`（红色）|

> 💡 预览表格只显示前 50 行，超出部分截断。

#### 步骤 4：查看异常记录（如有）

如果解析结果中有异常记录（`Disc.% > 0.01%`），会显示：

```
● 3 anomalies
[ View 3 Anomalous Rows ]  ← 点击此按钮
```

点击 **「View N Anomalous Rows」** 按钮：
1. 关闭上传弹窗
2. 主表格自动筛选，只显示异常记录（设置 `discRateFilter = 'gt0'`）
3. 异常行高亮显示（淡红色脉冲动画，10 秒后自动停止）

#### 步骤 5：确认并更新

1. 在上传弹窗中，点击右下角的 **「Confirm & Update」** 按钮
2. 系统并行调用 `updateMonthReport` API，逐条更新记录
3. 更新完成后，显示成功数量（如 `Successfully updated 12 records`）
4. 弹窗自动关闭，主表格刷新

> ⚠️ **注意**：
> - 只有 ID 非临时的记录才会被提交（临时 ID 格式：`temp_N`）
> - 提交字段：`scrub_revenue`、`scrub_payout`、`scrub_conversions`、`postback_conversions`、`cover`、`reason`
> - **不包含** `calc_revenue`、`calc_payout`（由后端计算）

---

## 9. 导出（Export）

### 9.1 操作步骤

1. 在 Finance 页面，点击筛选栏的 **「Export」** 按钮
2. 系统按当前筛选条件，请求 `exportMonthReport` API
3. 导出完成后，自动下载 Excel 文件（文件名：`report_YYYYMM_YYYYMM.xlsx`）

### 9.2 导出字段

导出文件包含以下字段（对应 `ALL_EXPORT_COLUMNS`）：

| 字段名 | 说明 |
|---------|------|
| Publisher | 渠道商 |
| Advertiser | 广告主 |
| Campaign | Campaign |
| Campaign ID | Campaign ID |
| Third Part ID | 第三方 Campaign ID |
| Revenue | 营收 |
| Scrub Rev. | 清洗后营收 |
| Confirm Rev. | 确认营收 |
| Payout | 成本 |
| Scrub. | 清洗抵扣 |
| Cover. | 补偿/抵扣 |
| Confirm Pay. | 确认成本 |
| Conversions | 转化数 |
| PB Conv. | Postback 转化数 |
| S Conv. | Scrub 转化数 |
| Reason | Scrub 原因 |
| AM Status | AM 确认状态 |
| PM Status | PM 确认状态 |

> 💡 **导出预设（Export Presets）**：
> 代码中预留了导出预设功能（通过 `exportPresets` 状态），可以保存常用的字段组合。
> 
> ⚠️ **待确认**：当前界面是否有预设管理 UI？还是需要手动修改代码调用？

---

## 10. 合计行（Total Row）

分组视图下，表格底部会自动显示**合计行**（Total Row），包含以下字段的汇总值：

| 字段 | 说明 |
|------|------|
| Revenue | 营收合计 |
| Scrub Revenue | 清洗后营收合计 |
| Confirm Revenue | 确认营收合计 |
| Payout | 成本合计 |
| Scrub | 清洗抵扣合计 |
| Cover | 补偿/抵扣合计 |
| Confirm Payout | 确认成本合计 |
| Conversions | 转化数合计 |
| PB Conv. | Postback 转化数合计 |

> 💡 合计数据优先使用后端返回的 `total_data`，如果没有则返回前端本地计算结果。

---

## 11. 异常记录高亮

### 11.1 自动检测异常

系统通过以下规则检测异常记录：

| 规则 | 说明 |
|------|------|
| `C.Revenue < C.Payout` | 确认应收 < 确认应付，平台亏损 |
| `Discrepancy Rate > 5%` | 差异率超过 5%（`(Conversions - PB Conv.) / PB Conv. × 100`）|
| `Reason` 非空 | 存在 Scrub 原因 |

### 11.2 高亮显示

异常记录会显示：
1. **红色脉冲动画**：行背景色淡红色脉冲（2 秒周期，无限循环）
2. **动画自动停止**：5 个周期（10 秒）后，动画停止，但背景色保留

> 💡 鼠标悬停异常行的 `Reason` 单元格，可以查看详细的 Scrub 原因描述。

---

## 12. 常见问题（FAQ）

### Q1：Scrub Rev. 和 Scrub 有什么区别？

**A**：
- `Scrub Rev.` = 广告主侧清洗抵扣金额（影响应收）
- `Scrub` = 渠道侧清洗抵扣金额（影响应付）

两者独立计算，但通常会按比例同步 Scrub。

### Q2：为什么我编辑了 Scrub Rev.，Scrub 也跟着变了？

**A**：这是系统的**联动逻辑**。在百分比模式下编辑 Scrub Rev.，系统会自动按相同比例更新 Scrub 和 S.Conv.。

如果不希望联动，请切换到**绝对值模式**编辑。

### Q3：为什么我无法 Confirm 某条记录？

**A**：请检查：
1. 是否有**后端权限**（部分账号可能只有只读权限）
2. 该记录是否**已被另一方确认**（如 AM 已确认，PM 才能确认）

### Q4：Upload Statement 解析后，为什么有些行显示 "Anomaly"？

**A**：`Anomaly` 标签表示 `Disc.% > 0.01%`，即广告主对账单数据与平台数据存在微小差异。

可能原因：
- 广告主拒绝部分转化（Fraud / CTIT）
- 后置归因导致转化数不一致
- 平台与广告主结算周期不同

### Q5：导出文件在哪里查看？

**A**：导出完成后，文件会自动下载到浏览器的默认下载目录。

文件名格式：`report_YYYYMM_YYYYMM.xlsx`（如 `report_202603_202603.xlsx`）。

---

## 附录：快捷操作索引

| 操作 | 入口 | 说明 |
|------|------|------|
| 快速进入 Finance | 任意页面按 `Ctrl+K` | 输入「Finance」|
| 上传对账单 | Finance → 「Upload Statement」| 支持 .xlsx/.xls/.csv |
| 导出数据 | Finance → 「Export」| 自动下载 Excel |
| 内联编辑字段 | 点击单元格 | 输入后回车保存 |
| 绝对值/百分比切换 | 悬停列标题 → 点击 ⇄% / ⇄Val | 切换输入模式 |
| 批量确认 | 勾选复选框 → 「Batch Confirm」| 支持 AM/PM 状态 |
| 批量调整 | 勾选复选框 → 「Batch Adjustment」| 支持 Scrub Rev./Scrub/S.Conv. |
| 分组视图切换 | 点击「Detail View」/「By Advertiser」等标签 | 动态显示/隐藏维度列 |
| 显示/隐藏状态列 | 点击「📊 Hide Status Columns」| 扩大数据列显示空间 |

---

**文档结束** 🎉
