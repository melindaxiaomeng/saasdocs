# 运营与用户操作手册
## 模块九：Resources（资源管理）

> **文档版本**：v1.0  
> **撰写日期**：2026-03-25  
> **目标读者**：内部运营人员、使用 SaaS 平台的客户  
> **前置要求**：无（独立功能模块）

---

## 目录

1. [模块概述](#1-模块概述)
2. [Campaign Label（Campaign 标签管理）](#2-campaign-labelcampaign-标签管理)
3. [Unified Account Vault（统一账号保管库）](#3-unified-account-vault统一账号保管库)
4. [Audiences Hub（人群管理中心）](#4-audiences-hub人群管理中心)
5. [常见问题 FAQ](#5-常见问题-faq)

---

## 1. 模块概述

**Resources** 是平台的资源管理模块，用于管理 Campaign 标签、广告平台账号和人群数据。

### 1.1 访问路径

| 子模块 | 导航路径 | 说明 |
|--------|----------|------|
| Campaign Label | 左侧菜单 → **Resources** → **Campaign Label** | Campaign 标签分类管理 |
| Unified Account Vault | 左侧菜单 → **Resources** → **Unified Account Vault** | 广告平台账号统一管理 |
| Audiences Hub | 左侧菜单 → **Resources** → **Audiences Hub** | 人群规则与上传文件管理 |

### 1.2 核心功能地图

```
Resources
├── Campaign Label
│   ├── Table List（左侧：标签分类列表）
│   └── Label List（右侧：标签列表）
│       ├── 新增 Table
│       ├── 编辑 Table
│       ├── 删除 Table
│       ├── 新增 Label
│       ├── 编辑 Label
│       └── 删除 Label
├── Unified Account Vault
│   ├── 账号列表（筛选、分页）
│   ├── 新增账号
│   ├── 编辑账号
│   ├── 删除账号
│   ├── 查看账号详情（Drawer）
│   └── 2FA 动态码（Popover）
└── Audiences Hub
    ├── 人群列表（筛选、分页）
    ├── 创建人群（规则创建 / 文件上传）
    ├── 查看绑定 Campaign
    ├── 编辑人群
    ├── 删除人群
    └── 状态切换（Active ↔ Paused ↔ Inactive）
```

---

## 2. Campaign Label（Campaign 标签管理）

### 2.1 功能说明

Campaign Label 用于管理 Campaign 的标签分类（Table）和标签（Label），帮助用户对 Campaign 进行分组和标记。

**核心概念**：
- **Table**：标签分类（如"行业分类"、"地区分类"）
- **Label**：具体标签（如"游戏"、"电商"、"北美"、"欧洲"）
- **关系**：一个 Table 下可以有多个 Label（一对多关系）

### 2.2 页面布局

**页面路径**：`Resources` → `Campaign Label`

**页面布局**：左右分栏
- **左侧**：Table List（标签分类列表）
- **右侧**：Label List（标签列表，显示选中 Table 下的所有 Label）

### 2.3 Table List（左侧面板）

**功能说明**：管理标签分类（Table）。

**Table 列表**：
- 显示所有 Table 的名称
- 当前选中的 Table 高亮显示（蓝色背景）
- 鼠标悬停时显示 **Edit** 和 **Delete** 按钮

**新增 Table**：
1. 点击左侧面板右上角 **Add** 按钮
2. 在弹窗中填写表单：

| 字段 | 说明 | 必填 |
|------|------|------|
| Table Name | 分类名称 | 是 |
| Description | 描述 | 否 |
| Color | 颜色 | 是（从预设颜色中选择） |

3. 点击 **Save** 保存

**编辑 Table**：
1. 鼠标悬停在 Table 上，点击 **Edit** 按钮（铅笔图标）
2. 在弹窗中修改表单字段
3. 点击 **Save** 保存

**删除 Table**：
1. 鼠标悬停在 Table 上，点击 **Delete** 按钮（垃圾桶图标）
2. 确认删除（提示：`Delete this table and all its labels?`）
3. 确认后删除（同时删除该 Table 下的所有 Label）

**颜色选择**：
- 预设 8 种颜色：`#22c55e`（绿）、`#3b82f6`（蓝）、`#f59e0b`（黄）、`#8b5cf6`（紫）、`#ec4899`（粉）、`#ef4444`（红）、`#06b6d4`（青）、`#64748b`（灰）
- 点击颜色圆圈选中，选中的颜色有边框和放大效果

### 2.4 Label List（右侧面板）

**功能说明**：管理选中 Table 下的所有标签（Label）。

**标签列表**：

| 字段 | 说明 | 备注 |
|------|------|------|
| Label Name | 标签名称 | 显示颜色圆圈 + 标签名称 |
| Description | 描述 | 标签的描述信息 |
| Operate | 操作 | 编辑、删除 |

**新增 Label**：
1. 点击右侧面板右上角 **Add Label** 按钮
2. 在弹窗中填写表单：

| 字段 | 说明 | 必填 |
|------|------|------|
| Table | 所属分类 | 是（下拉单选，默认选中当前 Table） |
| Label Name | 标签名称 | 是 |
| Description | 描述 | 否 |
| Color | 颜色 | 是（从预设颜色中选择） |

3. 点击 **Save** 保存

**注意事项**：
- 如果没有选中任何 Table，或 Tables 列表为空，**Add Label** 按钮禁用
- 新增 Label 时，**Table** 字段默认选中当前选中的 Table
- 编辑 Label 时，**Table** 字段禁用（不能修改所属分类）

**编辑 Label**：
1. 点击标签行的 **Edit** 按钮（蓝色铅笔图标）
2. 在弹窗中修改表单字段
3. 点击 **Save** 保存

**删除 Label**：
1. 点击标签行的 **Delete** 按钮（红色垃圾桶图标）
2. 确认删除（提示：`Delete this label?`）
3. 确认后删除

### 2.5 使用场景

**场景 1：按行业分类 Campaign**
1. 创建一个 Table：`Industry`（颜色：蓝色）
2. 在该 Table 下创建多个 Label：
   - `Gaming`（游戏）
   - `E-commerce`（电商）
   - `Finance`（金融）
3. 为每个 Campaign 打上对应的 Label

**场景 2：按地区分类 Campaign**
1. 创建一个 Table：`Region`（颜色：绿色）
2. 在该 Table 下创建多个 Label：
   - `North America`（北美）
   - `Europe`（欧洲）
   - `Asia`（亚洲）
3. 为每个 Campaign 打上对应的 Label

---

## 3. Unified Account Vault（统一账号保管库）

### 3.1 功能说明

Unified Account Vault 用于统一管理所有广告平台的账号信息，包括账号密码、Token、2FA 动态码等敏感信息。

**核心功能**：
- 账号信息的增删改查
- 2FA 动态码查看（倒计时、复制）
- 敏感信息掩码显示（Password、Token）
- 账号详情查看（Drawer）

### 3.2 账号列表

**页面路径**：`Resources` → `Unified Account Vault`

**筛选条件**：

| 筛选字段 | 说明 | 交互方式 |
|----------|------|----------|
| Name | 按 PID 筛选 | 输入框（支持回车触发） |
| Account | 按 Email 筛选 | 输入框（支持回车触发） |
| Status | 按状态筛选 | 下拉单选（All Status / Active / Disable） |

**表格字段**：

| 字段 | 说明 | 备注 |
|------|------|------|
| PID | 账号 ID | 显示状态圆点（绿色=Active，灰色=Disable）+ PID（蓝色可点击） |
| Email | 邮箱 | 账号的邮箱地址 |
| Password | 密码 | 掩码显示（••••••），点击眼睛图标显示/隐藏 |
| Token | Token | 掩码显示（••••••），点击眼睛图标显示/隐藏 |
| Bucket Name | 存储桶名称 | 账号的存储桶名称 |
| Operate | 操作 | View Code（2FA 按钮）、编辑、删除 |

**状态圆点颜色**：
- **Active**（status=1）：绿色圆点（`bg-green-500`）
- **Disable**（status=2）：灰色圆点（`bg-gray-400`）

**PID 点击事件**：
- 点击 PID（蓝色文字），右侧滑出 **Detail Drawer**（账号详情抽屉）
- 抽屉显示该账号的详细信息（见 [3.4 Detail Drawer](#34-detail-drawer账号详情抽屉)）

### 3.3 2FA 动态码（View Code 按钮）

**功能说明**：查看账号的 2FA 动态码（每 30 秒刷新一次）。

**使用步骤**：
1. 点击表格中的 **View Code** 按钮
2. 弹出 Popover（居中显示）
3. 查看动态码：
   - **倒计时圆圈**：SVG 圆圈，显示剩余秒数（30s 周期）
   - **6 位动态码**：显示当前动态码（如 `123 456`）
   - **复制提示**：点击动态码即可复制，显示"✓ 已复制"
4. 倒计时结束后自动刷新动态码

**倒计时圆圈**：
- 蓝色圆圈（剩余 >10s）
- 橙色圆圈（剩余 ≤10s，警示）
- 圆圈有进度动画（stroke-dashoffset 过渡）

**复制动态码**：
1. 点击 6 位动态码
2. 自动复制到剪贴板
3. 显示"✓ 已复制"提示（1.5 秒后消失）

**关闭 Popover**：
- 点击外部区域自动关闭
- 再次点击 **View Code** 按钮关闭

### 3.4 Detail Drawer（账号详情抽屉）

**功能说明**：右侧滑出抽屉，显示账号的详细信息。

**打开方式**：
- 点击表格中的 PID（蓝色文字）

**抽屉内容**：

| 字段 | 说明 | 交互方式 |
|------|------|----------|
| Email | 邮箱 | 纯文字显示 |
| View Code | 2FA 动态码 | 按钮（点击显示 Popover） |
| Password | 密码 | 掩码显示，支持显示/隐藏 + 复制 |
| Token | Token | 掩码显示，支持显示/隐藏 + 复制 |
| Bucket Name | 存储桶名称 | 掩码显示，支持显示/隐藏 + 复制 |

**MaskField 组件**：
- 默认掩码显示（• 遮挡）
- 点击 **眼睛图标** 显示/隐藏明文
- 点击 **复制图标** 复制明文（显示"Copied"提示）

**关闭抽屉**：
- 点击右上角 **X** 按钮
- 点击抽屉外部遮罩

### 3.5 新增/编辑账号

**新增账号**：
1. 点击页面右上角 **Add Account** 按钮
2. 在弹窗中填写表单：

| 字段 | 说明 | 必填 | 备注 |
|------|------|------|------|
| PID | 账号 ID | 是 | 唯一标识（如 `P1001`） |
| Email | 邮箱 | 否 | 账号的邮箱地址 |
| Password | 密码 | 否 | 支持眼睛图标显示/隐藏 |
| Token | Token | 否 | 支持眼睛图标显示/隐藏 |
| Bucket Name | 存储桶名称 | 否 | 账号的存储桶名称 |
| QR Code (Secret) | 二维码（密钥） | 否 | 点击上传二维码文件 |
| Status | 状态 | 是 | Active（启用）/ Disable（停用） |

3. 点击 **Save** 保存

**表单字段说明**：
- **PID**：账号的唯一标识，必填
- **Email**：账号的邮箱地址
- **Password**：账号密码，输入时默认掩码显示，点击眼睛图标显示明文
- **Token**：账号 Token，输入时默认掩码显示，点击眼睛图标显示明文
- **Bucket Name**：存储桶名称
- **QR Code (Secret)**：点击上传二维码文件（虚线框），显示文件名
- **Status**：账号状态（Active / Disable）

**编辑账号**：
1. 点击表格中的 **Edit** 按钮（铅笔图标）
2. 在弹窗中修改表单字段
3. 点击 **Save** 保存

**删除账号**：
1. 点击表格中的 **Delete** 按钮（垃圾桶图标）
2. 确认删除（提示：`Delete this account?`）
3. 确认后删除

---

## 4. Audiences Hub（人群管理中心）

### 4.1 功能说明

Audiences Hub 用于统一管理所有人群规则与上传文件，支持规则创建和文件上传两种方式。

**两种创建方式**：
- **Rule Engine**（规则创建）：通过规则条件创建人群
- **File Upload**（文件上传）：通过上传文件创建人群

### 4.2 人群列表

**页面路径**：`Resources` → `Audiences Hub`

**筛选条件**：

| 筛选字段 | 说明 | 交互方式 | 适用条件 |
|----------|------|----------|----------|
| Source | 按来源筛选 | 下拉单选（All Source / Rule Engine / File Upload） | 所有 |
| Event Name | 按事件名筛选 | 下拉单选（All Events + 事件列表） | 仅 Rule Engine |
| Package Name | 按包名筛选 | 输入框（支持回车触发） | 仅 Rule Engine |
| Key | 按 Key 筛选 | 输入框（支持回车触发） | 仅 File Upload |

**动态筛选逻辑**：
- 选择 **Rule Engine** 时，显示 **Event Name** 和 **Package Name** 筛选框
- 选择 **File Upload** 时，显示 **Key** 筛选框
- 选择 **All** 时，不显示专属筛选框

**表格字段**：

| 字段 | 说明 | 备注 |
|------|------|------|
| ID | 人群 ID | 显示 PID（如 `A1001`） |
| Name | 人群名称 | 人群的名称 |
| Key | 人群 Key | 人群的唯一标识 |
| Source | 来源 | Rule Engine（蓝色 badge）/ File Upload（绿色 badge） |
| Rules / Note | 规则/备注 | 显示规则条件或上传文件说明 |
| Volume | 量级 | 人群量级（如 `1.2M`、`850K`） |
| Status | 状态 | Active（绿色）/ Paused（黄色）/ Inactive（灰色），点击按钮切换 |
| Operate | 操作 | 查看绑定 Campaign（眼睛图标）、编辑、删除 |

**Source Badge 样式**：
- **Rule Engine**：蓝色背景（`bg-blue-50`），蓝色文字（`text-blue-600`）
- **File Upload**：绿色背景（`bg-green-50`），绿色文字（`text-green-600`）

**Status 按钮样式**：
- **Active**：绿色背景（`bg-green-50`），绿色文字（`text-green-600`），绿色边框
- **Paused**：黄色背景（`bg-yellow-50`），黄色文字（`text-yellow-600`），黄色边框
- **Inactive**：灰色背景（`bg-gray-100`），灰色文字（`text-gray-400`），灰色边框

**状态切换逻辑**：
- 点击 **Status** 按钮，状态按以下顺序循环切换：
  - Active → Paused
  - Paused → Inactive
  - Inactive → Active

### 4.3 创建人群

**操作步骤**：
1. 点击页面右上角 **创建人群** 按钮
2. 选择创建方式：
   - **规则创建**（蓝色边框）
   - **文件上传**（绿色边框）
3. 填写表单
4. 点击 **Save** 保存

#### 4.3.1 规则创建（Rule Engine）

**表单字段**：

| 字段 | 说明 | 必填 |
|------|------|------|
| Name | 人群名称 | 是 |
| Event Name | 事件名 | 是（下拉单选） |
| Package Name | 包名 | 是 |
| Country | 国家 | 是 |
| Date Type | 日期类型 | 是（Duration / Fixed Range） |
| Duration Days | 持续天数 | 否（Date Type = Duration 时必填） |
| Start Date | 开始日期 | 否（Date Type = Fixed Range 时必填） |
| End Date | 结束日期 | 否（Date Type = Fixed Range 时必填） |
| Status | 状态 | 是（Active / Paused / Inactive） |

**Event Name 选项**：
- App Install（应用安装）
- App Open（应用打开）
- Purchase（购买）
- Add to Cart（加入购物车）
- Sign Up（注册）
- Subscription（订阅）

**Date Type 说明**：
- **Duration**：按持续天数计算（如最近 30 天）
- **Fixed Range**：按固定日期范围计算（如 2024-01-01 至 2024-01-31）

#### 4.3.2 文件上传（File Upload）

**操作步骤**：
1. 选择 **文件上传** 方式
2. 点击 **上传文件** 按钮
3. 选择文件（支持 CSV、XLSX 等格式）
4. 系统自动读取文件名作为人群名称
5. 点击 **Save** 保存

**文件要求**：
- 文件格式：CSV、XLSX
- 文件大小：不超过 10MB
- 文件内容：包含人群数据（如设备 ID、用户 ID 等）

### 4.4 查看绑定 Campaign

**功能说明**：查看该人群绑定的所有 Campaign。

**操作步骤**：
1. 点击表格中的 **Eye** 按钮（眼睛图标）
2. 弹出 **Campaign View Modal**（模态框）
3. 查看绑定的 Campaign 列表
4. 点击 **Close** 关闭模态框

**模态框内容**：
- 人群名称
- 绑定的 Campaign 列表（显示 Campaign 名称）
- 如果没有绑定任何 Campaign，显示"No campaigns bound"

### 4.5 编辑人群

**操作步骤**：
1. 点击表格中的 **Edit** 按钮（铅笔图标）
2. 修改表单字段
3. 点击 **Save** 保存

**注意事项**：
- 编辑人群时，不能修改创建方式（Rule Engine / File Upload）
- 可以修改规则条件或上传新文件

### 4.6 删除人群

**操作步骤**：
1. 点击表格中的 **Delete** 按钮（垃圾桶图标）
2. 确认删除（提示：`Delete this audience?`）
3. 确认后删除

**注意事项**：
- 删除人群后，该人群将不再可用
- 建议先解除该人群与 Campaign 的绑定，再删除人群

---

## 5. 常见问题 FAQ

### Q1: Campaign Label 的 Table 和 Label 有什么区别？

**A**:
- **Table**：标签分类（如"行业分类"），用于组织和管理 Label
- **Label**：具体标签（如"游戏"、"电商"），用于标记 Campaign
- 一个 Table 下可以有多个 Label（一对多关系）

### Q2: Unified Account Vault 的 2FA 动态码怎么用？

**A**:
1. 点击 **View Code** 按钮
2. 查看弹出的动态码（每 30 秒刷新一次）
3. 点击动态码复制到剪贴板
4. 在广告平台登录时输入该动态码

### Q3: Audiences Hub 的 Rule Engine 和 File Upload 有什么区别？

**A**:
- **Rule Engine**（规则创建）：通过规则条件创建人群（如"年龄 25-40，消费 >$200"）
- **File Upload**（文件上传）：通过上传文件创建人群（如上传设备 ID 列表）

### Q4: 如何快速找到某个账号？

**A**:
使用筛选条件：
1. 在 **Name** 输入框中输入 PID（如 `P1001`）
2. 在 **Account** 输入框中输入 Email（如 `user@example.com`）
3. 在 **Status** 下拉框中选择状态（Active / Disable）
4. 点击 **Search** 按钮（自动触发）

### Q5: Audiences Hub 的状态切换逻辑是什么？

**A**:
点击 **Status** 按钮，状态按以下顺序循环切换：
- Active → Paused
- Paused → Inactive
- Inactive → Active

---

## 附录：API 接口清单

| 功能 | 接口 | 方法 | 说明 |
|------|------|------|------|
| Campaign Label - Tables | `/api/v1/campaign-label/tables/` | GET | 获取 Table 列表 |
| Campaign Label - Tables | `/api/v1/campaign-label/tables/` | POST | 新增 Table |
| Campaign Label - Tables | `/api/v1/campaign-label/tables/{id}/` | PUT | 更新 Table |
| Campaign Label - Tables | `/api/v1/campaign-label/tables/{id}/` | DELETE | 删除 Table |
| Campaign Label - Labels | `/api/v1/campaign-label/children/` | GET | 获取 Label 列表 |
| Campaign Label - Labels | `/api/v1/campaign-label/` | POST | 新增 Label |
| Campaign Label - Labels | `/api/v1/campaign-label/{id}/` | PUT | 更新 Label |
| Campaign Label - Labels | `/api/v1/campaign-label/{id}/` | DELETE | 删除 Label |
| Unified Account Vault | `/pids` | GET | 获取账号列表 |
| Unified Account Vault | `/pids` | POST | 新增账号 |
| Unified Account Vault | `/pids/{id}` | DELETE | 删除账号 |
| Unified Account Vault | `/pids/{id}/mfa-code` | GET | 获取 2FA 动态码 |
| Audiences Hub | `/api/v1/audiences/` | GET | 获取人群列表 |
| Audiences Hub | `/api/v1/audiences/` | POST | 创建人群 |
| Audiences Hub | `/api/v1/audiences/{id}/` | PUT | 更新人群 |
| Audiences Hub | `/api/v1/audiences/{id}/` | DELETE | 删除人群 |

---

**文档结束**

> **下一步**：完成模块九后，建议编写 **README 文档**，整合所有模块，方便用户快速查阅。
