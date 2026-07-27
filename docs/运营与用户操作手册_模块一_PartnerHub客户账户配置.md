# 运营与用户操作手册

## 模块一：Partner Hub 客户账户配置

> **适用角色**：内部运营（BD/PM/Admin）、客户（Advertiser / Publisher）
>
> **系统入口**：左侧导航 → `Workplace` → `Partner Hub`
>
> **版本**：v1.0 | **生效日期**：2026-06-18

---

## 1. 概述

Partner Hub 是整个 SaaS 平台的客户账户管理中心，负责：

- **Advertiser（广告主）**：创建/编辑广告主账户，配置 Postback、财务信息、流量过滤规则
- **Publisher（渠道商）**：创建/编辑渠道账户，配置 Track URL、Postback、转化上限、CTIT/CTPT 参数
- **Publisher Global Rules**：配置广告主与渠道之间的全局投放规则
- **Campaign List**：查看客户旗下的 Campaign 列表
- **Block Campaign / Block Publisher**：管理屏蔽关系

---

## 2. Advertiser（广告主）配置

### 2.1 入口与界面

**入口**：`Partner Hub` → 左侧列表切换到 `Advertiser List` Tab

**列表字段说明**：

| 字段 | 说明 |
|------|------|
| Name (ID) | 广告主名称 + 系统 ID，格式：`名称(ID)` |
| Status 徽章 | `Active`（绿色）/ `Inactive`（灰色） |
| Auto 标签 | 系统自动创建标识 |

**操作按钮（每条记录右侧）**：

| 按钮 | 说明 |
|------|------|
| ⚙️ Config | 打开广告主配置抽屉（Drawer），可编辑全部字段 |
| 🔗 | 打开 Publisher Global Rules，配置该广告主与渠道的关联规则 |

---

### 2.2 创建广告主

1. 点击页面右上角 **`+ New Advertiser`** 按钮
2. 系统打开右侧 Drawer（抽屉），填写以下字段
3. 点击 **`Save`** 提交

#### 字段填写说明

**① 基础信息**

| 字段 | 必填 | 说明 |
|------|-----|------|
| Advertiser ID | 系统自动生成 | 保存后自动生成，不可修改 |
| Name | ✅ | 广告主公司名称或品牌名 |
| Status | ✅ | `Active` / `Inactive` / `Pending`（新建默认 Active） |
| BD | ✅ | 商务负责人，从用户列表中选择 |
| AM | ✅ | 账户管理员（Advertiser Manager），从用户列表中选择 |

**② 联系信息**

| 字段 | 说明 |
|------|------|
| User Name | 联系人姓名 |
| Email | 联系邮箱 |
| Notification Email | 接收系统通知的邮箱（可与 Email 不同） |
| Memo | 内部备注，仅内部运营可见 |

**③ API & Postback 配置**

| 字段 | 说明 |
|------|------|
| Postback URL | 接收转化回调的 URL 地址 |
| API | 广告主侧 API 地址（用于拉取报表等） |

**④ 财务信息**

| 字段 | 说明 |
|------|------|
| Payment Type | `Invoice needed`（需要发票）/ `Invoice free`（无需发票） |
| Payment Terms | 账期说明（如 Net 30），纯文本字段 |
| Beneficial Name | 收款账户受益人姓名 |
| Bank Name | 银行名称 |
| SWIFT Code | 银行 SWIFT 代码 |
| Address | 公司注册地址 |
| Margin | 毛利率（数值，用于内部核算） |

**⑤ 公司信息**

| 字段 | 说明 |
|------|------|
| Company Name | 公司全称 |
| Company Nick Name | 公司简称 |

**⑥ 限额配置**

| 字段 | 说明 |
|------|------|
| Daily Conversion Caps | 单日转化上限（数值） |
| Daily Total Adv Click Caps | 单日点击上限（数值） |

**⑦ IP / Geo 过滤**

| 字段 | 说明 |
|------|------|
| IP Whitelist | 白名单 IP，逗号分隔 |
| Geo Filter | 允许的国家代码，多选（从系统支持的国家列表中选择，如 `US`, `JP`） |

**⑧ 高级设置**

| 字段 | 类型 | 说明 |
|------|------|------|
| Block Channel | 文本 | 屏蔽的 Channel 标签（逗号分隔） |
| White Channels | 文本 | 白名单 Channel 标签（逗号分隔） |
| Package Block | 文本 | 屏蔽的包名（逗号分隔） |
| Campaign ID Filter | 文本 | Campaign ID 过滤规则 |
| Weget Min / Weget Max | 数值 | Weget 控制区间（最小/最大值） |
| Auto Test | 开关 | 是否启用自动测试 |
| More Tracking | 开关 | 是否启用更多追踪参数 |
| Short ClickID | 开关 | 是否使用短 ClickID |
| Reject Cap Must | 开关 | 超限是否强制拒绝 |

**⑨ 合同信息**

| 字段 | 说明 |
|------|------|
| Contract ID | 合同编号 |
| Contract Valid To | 合同有效期截止日期 |

---

### 2.3 编辑广告主

1. 在 Advertiser List 中点击目标广告主卡片
2. 点击卡片右侧的 **`⚙️ Config`** 按钮
3. 右侧 Drawer 打开，所有字段均可编辑
4. 修改后点击 **`Save`** 保存

> **提示**：`Advertiser ID` 不可修改。状态切换 `Active` ↔ `Inactive` 即时生效。

---

### 2.4 状态管理

| 状态 | 说明 | 可进行的操作 |
|------|------|------|
| Active | 正常可用 | 正常投放、查看报表 |
| Inactive | 停用 | 无法新建 Campaign，现有 Campaign 暂停 |
| Pending | 待审核（如启用） | 视业务配置而定 |

状态切换：在 Config Drawer 顶部下拉框直接切换，Save 后生效。

---

## 3. Publisher（渠道商）配置

### 3.1 入口与界面

**入口**：`Partner Hub` → 左侧列表切换到 `Publisher List` Tab

**列表字段说明**：与 Advertiser List 一致（Name + ID + Status 徽章 + Config / Global Rules 按钮）

---

### 3.2 创建渠道商

1. 点击页面右上角 **`+ New Publisher`** 按钮
2. 系统打开右侧 Drawer，填写字段
3. 点击 **`Save`** 提交

> **注意**：新建 Publisher 时，`Key`（Token）和 `Request URL` 由系统自动生成，保存后显示在表单中，可一键复制。

#### 字段填写说明

**① 基础信息**

| 字段 | 必填 | 说明 |
|------|-----|------|
| Publisher ID | 系统自动生成 | 保存后生成 |
| Name | ✅ | 渠道商名称 |
| User Name |  | 联系人姓名 |
| Email |  | 联系邮箱 |
| Password |  | 登录密码（新建时设置，编辑时显示 `*` 掩码） |
| Notification Email |  | 接收通知的邮箱 |
| Status | ✅ | `Active` / `Inactive` |
| BD |  | 对接商务（从用户列表多选） |
| PM |  | 渠道经理（从用户列表多选） |

**② Track & Postback 配置**

| 字段 | 说明 |
|------|------|
| Key（Token） | 系统自动生成的身份令牌，可一键复制 |
| Request URL | 系统自动生成的流量请求地址，可一键复制 |
| Postback URL | 接收转化回调的地址 |
| Postback Type | `Install`（按安装回调）/ `Payable`（按付费事件回调） |
| Optimize URL | 优化事件回调地址 |
| Event Postback URL | 额外事件 Postback 地址 |
| Reject Event URL | 拒绝事件的回调地址 |

**③ Channel 配置**

| 字段 | 说明 |
|------|------|
| Channel Tag | 渠道标签（用于流量标识） |
| Campaign Rsteps Filter | 允许回传的转化步骤（多选：Step 1 / Step 2 / Step 3...） |

**④ 限额配置**

| 字段 | 说明 |
|------|------|
| Click Caps | 单日点击上限 |
| Conversion Caps | 单日转化上限 |
| Cam Channel Conv Limit | 单 Channel 转化上限 |
| Cam Conv Limit | 单 Campaign 转化上限 |
| Margin | 毛利率（数值） |

**⑤ 屏蔽配置**

| 字段 | 说明 |
|------|------|
| Block Channel | 屏蔽的 Channel（逗号分隔） |
| White Channel | 白名单 Channel（逗号分隔） |
| Block Package Name | 屏蔽的包名（逗号分隔） |

**⑥ CTIT / CTCPT 配置**

| 字段 | 说明 |
|------|------|
| Min CTIT / Max CTIT | 点击→安装时间窗口（秒），超出范围的转化可被过滤 |
| Min CTCPT / Max CTCPT | 点击→付费时间窗口（秒） |
| CTIT/CTCPT Mode | `Default` / `NON Default`（非默认模式） |

**⑦ 开关选项**

| 字段 | 说明 |
|------|------|
| Exclude Global Cap | 是否排除全局上限（勾选后不受平台全局 Cap 限制） |
| Track HTTPS | 是否使用 HTTPS 追踪链接 |
| Is Testing | 是否测试模式 |
| Redirect To Smartlink | 是否重定向到 Smartlink |
| Internal Click | 是否计为内部点击 |

**⑧ 财务 & 合同信息**

（字段与 Advertiser 相同：Contract ID、Contract Valid To、Company Name、Company Nick Name、Payment Terms、Payment Type、Beneficial Name、Bank Name、SWIFT Code、Address）

---

### 3.3 一键复制 Token / Request URL

在 Publisher Config Drawer 中：

1. 保存后，`Key` 和 `Request URL` 字段右侧会出现复制按钮
2. 点击复制按钮，系统自动写入剪贴板
3. 将 `Request URL` 提供给渠道集成 SDK，`Key` 用于身份验证

---

## 4. Publisher Global Rules（广告主-渠道关联规则）

### 4.1 入口

有以下两种入口均可进入：

1. 在 Advertiser List 中点击某广告主卡片 → 点击 **`🔗`** 按钮
2. 在 Publisher List 中点击某渠道卡片 → 点击 **`🔗`** 按钮

### 4.2 功能说明

Publisher Global Rules 用于配置 **广告主与渠道之间的全局投放规则**，包括：

- 允许该广告主与哪些渠道合作
- 全局 Payout 上浮/下浮比例
- 全局 Cap 配置
- 屏蔽规则（全局生效）

> ⚠️ **注意**：具体可配置的规则项以系统实际显示的字段为准。不同广告主/渠道组合可配置不同的全局规则。

### 4.3 操作步骤

1. 打开 Publisher Global Rules 页面
2. 系统显示当前广告主已关联的渠道列表
3. 点击 **`+ Add Rule`** 新增关联规则
4. 选择渠道，配置对应规则参数
5. 点击 **`Save`** 保存

---

## 5. Campaign List（Campaign 管理）

### 5.1 入口

在 Partner Hub 主界面，选中某个 Advertiser 或 Publisher 后，右侧切换到 **`Campaign List`** Tab。

### 5.2 功能说明

Campaign List 展示当前选中客户旗下的所有 Campaign，字段包括：

| 字段 | 说明 |
|------|------|
| Campaign ID | 系统 Campaign ID |
| T-Campaign ID | 第三方 Campaign ID |
| Name | Campaign 名称 |
| Package Name | 应用包名 |
| Country | 投放国家 |
| OS | 操作系统（iOS / Android） |
| Payout Mode | 计费模式（CPI / CPA 等） |
| Payout | 单价 |
| Status | `Active` / `Inactive` |
| Labels | 标签（Hot / Top 等） |

### 5.3 快捷操作

- 点击 Campaign 行可查看详情
- 点击 **`Edit`** 按钮可跳转到 Campaign 编辑页面
- 点击 **`KPI`** 按钮可查看该 Campaign 的 KPI 仪表板

---

## 6. Block Campaign / Block Publisher

### 6.1 Block Campaign（屏蔽 Campaign）

**功能**：阻止特定 Campaign 向特定渠道发量。

**操作步骤**：

1. 在 Partner Hub 主界面，切换到 **`Block Campaign`** Tab
2. 点击 **`+ Add Block`**
3. 选择需要屏蔽的 Campaign
4. 选择被屏蔽的 Publisher
5. 设置屏蔽原因（可选）
6. 点击 **`Save`**

### 6.2 Block Publisher（屏蔽渠道）

**功能**：阻止特定广告主与特定渠道合作。

**操作步骤**：

1. 切换到 **`Block Publisher`** Tab
2. 点击 **`+ Add Block`**
3. 选择 Advertiser 和 Publisher
4. 设置屏蔽原因（可选）
5. 点击 **`Save`**

---

## 7. 常见问题（FAQ）

| 问题 | 解答 |
|------|------|
| 新建广告主后 ID 在哪里查看？ | 保存后 Drawer 顶部显示 `Advertiser ID`，也可在列表卡片上看到 `名称(ID)` |
| Publisher 的 Key/Token 在哪里获取？ | 创建后打开 Config Drawer，在 `Key` 字段右侧点击复制按钮 |
| 如何批量修改多个广告主的 BD？ | 目前需逐个编辑，暂不支持批量修改 |
| Status 改为 Inactive 后正在投放的 Campaign 会怎样？ | Campaign 会被暂停，状态需手动改回 Active 才能恢复 |
| Postback URL 支持动态宏参数吗？ | 支持，具体宏参数格式请咨询技术对接同事 |

---

*本文档基于 saas_new 项目 PartnerHub.jsx（2026-06-18 版本）真实字段编写，严禁外传。如有功能更新，以系统实际界面为准。*
