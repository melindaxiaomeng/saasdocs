# 运营与用户操作手册
## 模块八：User Management（用户管理）

> **文档版本**：v1.0  
> **撰写日期**：2026-03-25  
> **目标读者**：内部运营人员、系统管理员、使用 SaaS 平台的客户  
> **前置要求**：已完成模块一（Partner Hub）的配置，了解基本的用户管理概念

---

## 目录

1. [模块概述](#1-模块概述)
2. [Users（用户管理）](#2-users用户管理)
3. [Departments（部门管理）](#3-departments部门管理)
4. [Positions（职务管理）](#4-positions职务管理)
5. [权限配置详解](#5-权限配置详解)
6. [常见问题 FAQ](#6-常见问题-faq)

---

## 1. 模块概述

**User Management** 是平台的用户权限管理中心，用于管理系统用户、部门结构、职务体系和权限配置。

### 1.1 访问路径

| 子模块 | 导航路径 | 说明 |
|--------|----------|------|
| Users | 左侧菜单 → **User Management** → **Users** Tab | 用户列表与权限管理 |
| Departments | 左侧菜单 → **User Management** → **Departments** Tab | 部门结构管理 |
| Positions | 左侧菜单 → **User Management** → **Positions** Tab | 职务体系管理 |

### 1.2 核心功能地图

```
User Management
├── Users（用户管理）
│   ├── 用户列表（筛选、搜索、分页）
│   ├── 新增用户
│   ├── 编辑用户
│   ├── 删除用户
│   ├── 重置密码
│   ├── 权限配置
│   │   ├── Menu Permissions（菜单权限）
│   │   ├── Functional Permissions（功能权限）
│   │   └── Resource（资源权限）
│   └── 状态切换（Active ↔ Inactive）
├── Departments（部门管理）
│   ├── 部门列表
│   ├── 新增部门
│   ├── 编辑部门
│   ├── 删除部门
│   └── 状态切换
└── Positions（职务管理）
    ├── 职务列表
    ├── 新增职务
    ├── 编辑职务
    ├── 删除职务
    └── 状态切换
```

---

## 2. Users（用户管理）

### 2.1 功能说明

Users Tab 用于管理系统中的所有用户，包括用户的增删改查、密码重置和权限配置。

### 2.2 用户列表

**页面路径**：`User Management` → `Users` Tab

**筛选条件**：

| 筛选字段 | 说明 | 交互方式 |
|----------|------|----------|
| Search | 全局搜索 | 输入框（支持用户名、姓名、邮箱搜索，回车触发） |
| Status | 按状态筛选 | 下拉单选（All Status / Active / Inactive） |

**表格字段**：

| 字段 | 说明 | 备注 |
|------|------|------|
| User | 用户 | 显示头像（用户名首字母）+ 用户名 + 真实姓名 |
| Email | 邮箱 | 用户的邮箱地址 |
| Department | 部门 | 显示部门名称（badge 样式，最多显示 2 个，超出显示 +N） |
| Position | 职务 | 显示职务名称（badge 样式，最多显示 2 个，超出显示 +N） |
| Role | 角色 | Super Admin（黄色 badge）/ User（蓝色 badge） |
| Status | 状态 | Active（绿色）/ Inactive（红色），点击按钮切换 |
| Last Login | 最后登录时间 | 显示日期，未登录显示 "-" |
| Actions | 操作 | 编辑、重置密码、权限配置、删除 |

**用户头像生成规则**：
- 使用用户名首字母（大写）作为头像
- 背景色为蓝色（`#3B82F6`）
- 圆形头像（36×36px）

**部门/职务 badge 样式**：
- 部门：紫色背景（`#EEF2FF`），紫色文字（`#6366F1`）
- 职务：黄色背景（`#FEF3C7`），橙色文字（`#D97706`）

**角色 badge 样式**：
- Super Admin：黄色背景（`#FEF3C7`），橙色文字（`#D97706`）
- User：蓝色背景（`#E0E7FF`），靛蓝色文字（`#4F46E5`）

### 2.3 新增用户

**操作步骤**：
1. 点击页面右上角 **Add User** 按钮
2. 填写表单：

| 字段 | 说明 | 必填 | 备注 |
|------|------|------|------|
| Name | 用户名 | 是 | 登录用户名，唯一标识 |
| Password | 密码 | 是 | 至少 6 个字符 |
| Real Name | 真实姓名 | 否 | 用户真实姓名 |
| Email | 邮箱 | 否 | 用户的邮箱地址 |
| Mobile | 手机号 | 否 | 用户的手机号码 |
| Status | 状态 | 是 | Active（启用）/ Inactive（停用），默认 Active |
| Is Superuser | 是否超级管理员 | 是 | 0（普通用户）/ 1（超级管理员），默认 0 |
| Department | 部门 | 否 | 下拉多选（从部门列表选择） |
| Position | 职务 | 否 | 下拉多选（从职务列表选择） |

3. 点击 **Save** 保存

**注意事项**：
- **Name** 和 **Password** 为必填项
- **Password** 至少 6 个字符
- **Is Superuser** = 1 的用户拥有所有权限，无需单独配置权限

### 2.4 编辑用户

**操作步骤**：
1. 点击表格中的 **Edit** 按钮（铅笔图标）
2. 修改表单字段
3. 点击 **Save** 保存

**编辑时的差异**：
- 编辑时不需要填写密码（密码通过重置密码功能修改）
- 表单会自动回填当前用户的信息

### 2.5 删除用户

**操作步骤**：
1. 点击表格中的 **Delete** 按钮（垃圾桶图标）
2. 确认删除（弹窗提示：`Are you sure to delete user "xxx"?`）
3. 确认后删除

**注意事项**：
- 删除用户后，该用户将无法登录系统
- 建议先将会话状态改为 Inactive，而不是直接删除

### 2.6 重置密码

**操作步骤**：
1. 点击表格中的 **Key** 按钮（钥匙图标）
2. 在弹窗中输入新密码（至少 6 个字符）
3. 点击 **Save** 保存

**密码要求**：
- 至少 6 个字符
- 不支持特殊字符限制（由后端验证）

### 2.7 状态切换

**操作步骤**：
1. 点击表格中的 **Status** 按钮
2. 状态在 Active ↔ Inactive 之间切换

**状态说明**：
- **Active**：用户可以正常登录系统
- **Inactive**：用户无法登录系统（账号停用）

### 2.8 权限配置

**功能说明**：为每个用户配置菜单权限、功能权限和资源权限。

**操作步骤**：
1. 点击表格中的 **Shield** 按钮（盾牌图标）
2. 在下拉菜单中选择权限类型：
   - **Menu Permissions**：菜单权限
   - **Functional Permissions**：功能权限
   - **Resource**：资源权限
3. 在弹窗中配置权限
4. 点击 **Save Permissions** 保存

**权限配置详解**：见 [第 5 章：权限配置详解](#5-权限配置详解)

---

## 3. Departments（部门管理）

### 3.1 功能说明

Departments Tab 用于管理公司的部门结构，支持部门的增删改查和状态管理。

### 3.2 部门列表

**页面路径**：`User Management` → `Departments` Tab

**筛选条件**：
- Search（输入框，按部门名称搜索）

**表格字段**：

| 字段 | 说明 | 备注 |
|------|------|------|
| Name | 部门名称 | 显示部门图标（紫色）+ 部门名称 |
| Code | 部门编码 | 部门的唯一编码 |
| Description | 描述 | 部门的描述信息 |
| Status | 状态 | Active（绿色）/ Inactive（红色），点击按钮切换 |
| Actions | 操作 | 编辑、删除 |

**部门图标**：
- 使用 `Building2` 图标
- 背景色为紫色（`#EEF2FF`）
- 图标颜色为靛紫色（`#6366F1`）

### 3.3 新增部门

**操作步骤**：
1. 点击页面右上角 **Add Department** 按钮
2. 填写表单：

| 字段 | 说明 | 必填 |
|------|------|------|
| Name | 部门名称 | 是 |
| Code | 部门编码 | 是（唯一标识） |
| Description | 描述 | 否 |
| Status | 状态 | 是（Active / Inactive，默认 Active） |

3. 点击 **Save** 保存

**注意事项**：
- **Name** 和 **Code** 为必填项
- **Code** 必须唯一，不能与已有部门重复

### 3.4 编辑部门

**操作步骤**：
1. 点击表格中的 **Edit** 按钮
2. 修改表单字段
3. 点击 **Save** 保存

### 3.5 删除部门

**操作步骤**：
1. 点击表格中的 **Delete** 按钮
2. 确认删除（弹窗提示：`Are you sure to delete department "xxx"?`）
3. 确认后删除

**注意事项**：
- 删除部门后，该部门下的用户将不再关联部门
- 建议先删除部门下的用户，或将会话状态改为 Inactive

### 3.6 状态切换

**操作步骤**：
1. 点击表格中的 **Status** 按钮
2. 状态在 Active ↔ Inactive 之间切换

---

## 4. Positions（职务管理）

### 4.1 功能说明

Positions Tab 用于管理公司的职务体系，支持职务的增删改查和状态管理。

### 4.2 职务列表

**页面路径**：`User Management` → `Positions` Tab

**筛选条件**：
- Search（输入框，按职务名称搜索）

**表格字段**：

| 字段 | 说明 | 备注 |
|------|------|------|
| Name | 职务名称 | 显示职务图标（橙色）+ 职务名称 |
| Code | 职务编码 | 职务的唯一编码 |
| Description | 描述 | 职务的描述信息 |
| Status | 状态 | Active（绿色）/ Inactive（红色），点击按钮切换 |
| Actions | 操作 | 编辑、删除 |

**职务图标**：
- 使用 `Briefcase` 图标
- 背景色为黄色（`#FEF3C7`）
- 图标颜色为橙色（`#D97706`）

### 4.3 新增职务

**操作步骤**：
1. 点击页面右上角 **Add Position** 按钮
2. 填写表单：

| 字段 | 说明 | 必填 |
|------|------|------|
| Name | 职务名称 | 是 |
| Code | 职务编码 | 是（唯一标识） |
| Description | 描述 | 否 |
| Status | 状态 | 是（Active / Inactive，默认 Active） |

3. 点击 **Save** 保存

**注意事项**：
- **Name** 和 **Code** 为必填项
- **Code** 必须唯一，不能与已有职务重复

### 4.4 编辑职务

**操作步骤**：
1. 点击表格中的 **Edit** 按钮
2. 修改表单字段
3. 点击 **Save** 保存

### 4.5 删除职务

**操作步骤**：
1. 点击表格中的 **Delete** 按钮
2. 确认删除（弹窗提示：`Are you sure to delete position "xxx"?`）
3. 确认后删除

**注意事项**：
- 删除职务后，该职务下的用户将不再关联职务
- 建议先删除职务下的用户，或将会话状态改为 Inactive

### 4.6 状态切换

**操作步骤**：
1. 点击表格中的 **Status** 按钮
2. 状态在 Active ↔ Inactive 之间切换

---

## 5. 权限配置详解

### 5.1 功能说明

权限配置是 User Management 的核心功能，用于控制用户可以访问哪些菜单、使用哪些功能和查看哪些数据。

**三种权限类型**：
1. **Menu Permissions**（菜单权限）：控制用户可以访问哪些菜单页面
2. **Functional Permissions**（功能权限）：控制用户可以使用哪些功能操作
3. **Resource**（资源权限）：控制用户可以查看哪些数据资源

### 5.2 Menu Permissions（菜单权限）

**功能说明**：配置用户可以访问的菜单页面。

**配置界面**：
- 左侧：**Menu Collections**（菜单权限集合）
- 右侧：**Individual Menus**（独立菜单选择）

**Menu Collections（菜单权限集合）**：
- 预设的菜单权限模板（如"运营人员菜单"、"财务人员菜单"）
- 勾选后快速分配一组菜单权限
- 支持多选

**Individual Menus（独立菜单选择）**：
- 菜单树结构，支持父子级联动
- 勾选父级菜单时，自动勾选所有子级菜单
- 部分勾选时显示"Partial"状态

**菜单树显示规则**：
- 一级菜单：无缩进
- 二级菜单：缩进 20px
- 三级菜单：缩进 40px
- 已选菜单：蓝色背景（`#EFF6FF`）+ 蓝色边框

**保存权限**：
1. 配置完成后，点击右下角 **Save Permissions** 按钮
2. 系统同时保存 Menu Collections 和 Individual Menus 的配置
3. 保存成功后提示："Permissions saved successfully"

### 5.3 Functional Permissions（功能权限）

**功能说明**：配置用户可以使用的功能操作。

**配置界面**：
- 左侧：**Permission Collections**（功能权限集合）
- 右侧：**Individual Permissions**（独立功能权限选择）

**Permission Collections（功能权限集合）**：
- 预设的功能权限模板（如"财务报表权限"、"Campaign 管理权限"）
- 勾选后快速分配一组功能权限
- 支持多选

**Individual Permissions（独立功能权限选择）**：
- 列出所有功能权限
- 每个权限显示：**权限名称** + **权限编码**
- 勾选需要的功能权限

**功能权限显示格式**：
```
权限名称
权限编码
```

**保存权限**：
1. 配置完成后，点击右下角 **Save Permissions** 按钮
2. 系统同时保存 Permission Collections 和 Individual Permissions 的配置
3. 保存成功后提示："Permissions saved successfully"

### 5.4 Resource（资源权限）

**功能说明**：配置用户可以查看的数据资源。

**配置界面**：
- 左右分栏布局
- 左侧：**Unauthorized**（未授权资源）
- 右侧：**Authorized**（已授权资源）

**Unauthorized（未授权资源）**：
- 显示当前用户未授权的资源列表
- 表格字段：
  - 复选框（支持全选/取消全选）
  - Name（资源名称）
  - Type（资源类型）
- 支持多选（勾选需要授权的资源）

**Authorized（已授权资源）**：
- 显示当前用户已授权的资源列表
- 表格字段：
  - Scope Name（资源范围名称）
  - Type（资源类型）
  - Status（状态：Active / Inactive）
  - Action（操作：删除授权）
- 支持分页

**授权操作**：
1. 在左侧 **Unauthorized** 列表中勾选需要授权的资源
2. 点击右下角 **Save** 按钮
3. 系统将选中的资源授权给用户
4. 授权成功后，资源从左侧移动到右侧

**撤销授权**：
1. 在右侧 **Authorized** 列表中找到需要撤销的资源
2. 点击 **Delete** 按钮（垃圾桶图标）
3. 确认撤销（提示：`Revoke this resource scope?`）
4. 确认后撤销授权

**覆盖式授权逻辑**：
- 保存时采用覆盖式授权（不是增量式）
- 即：保存后，用户的资源权限 = 原有授权 + 新增授权（去重）
- 不会删除原有授权（除非手动撤销）

---

## 6. 常见问题 FAQ

### Q1: 超级管理员（Super Admin）需要配置权限吗？

**A**: 不需要。超级管理员拥有所有权限，无需单独配置菜单权限、功能权限和资源权限。

### Q2: 用户登录后看不到某些菜单，怎么办？

**A**: 检查该用户的菜单权限配置：
1. 进入 `User Management` → `Users` Tab
2. 找到该用户，点击 **Shield** 按钮
3. 选择 **Menu Permissions**
4. 勾选需要的菜单权限
5. 点击 **Save Permissions** 保存

### Q3: 如何快速给多个用户配置相同的权限？

**A**: 使用权限集合（Permission Collections）：
1. 先创建一个权限集合（需要在权限管理页面创建）
2. 在配置用户权限时，勾选该权限集合
3. 点击 **Save Permissions** 保存
4. 其他用户也勾选相同的权限集合

### Q4: 部门和工作有什么关系？

**A**:
- **Department**（部门）：用于组织用户的结构（如"技术部"、"财务部"）
- **Position**（职务）：用于定义用户的职位（如"经理"、"专员"）
- 一个用户可以同时属于多个部门和多个职务

### Q5: 如何禁用一个用户账号？

**A**: 有两种方式：
1. **状态切换**：在用户列表中，点击 **Status** 按钮，切换为 Inactive
2. **删除用户**：点击 **Delete** 按钮，删除用户（不可逆）

建议使用方式 1（状态切换），而不是直接删除。

### Q6: 资源权限的"覆盖式授权"是什么意思？

**A**: 覆盖式授权是指：
- 保存时，系统会将"原有授权"和"新增授权"合并（去重）
- 不是增量式（只添加新授权，不删除旧授权）
- 也不是替换式（删除所有旧授权，只保留新授权）
- 而是：保留原有授权，添加新的授权

**示例**：
- 原有授权：Resource A、Resource B
- 新增授权：Resource B、Resource C
- 保存后：Resource A、Resource B、Resource C（去重）

---

## 附录：API 接口清单

| 功能 | 接口 | 方法 | 说明 |
|------|------|------|------|
| 用户列表 | `/api/v1/users/` | GET | 获取用户列表 |
| 新增用户 | `/api/v1/users/` | POST | 新增用户 |
| 更新用户 | `/api/v1/users/{id}/` | PUT | 更新用户信息 |
| 删除用户 | `/api/v1/users/{id}/` | DELETE | 删除用户 |
| 更新用户状态 | `/api/v1/users/{id}/status/` | PUT | 更新用户状态 |
| 重置密码 | `/api/v1/users/{id}/reset-password/` | POST | 重置用户密码 |
| 用户菜单权限 | `/api/v1/users/{id}/menus/` | GET | 获取用户菜单权限 |
| 设置用户菜单权限 | `/api/v1/users/{id}/menus/` | POST | 设置用户菜单权限 |
| 用户功能权限 | `/api/v1/users/{id}/permissions/` | GET | 获取用户功能权限 |
| 设置用户功能权限 | `/api/v1/users/{id}/permissions/` | POST | 设置用户功能权限 |
| 用户菜单权限集合 | `/api/v1/users/{id}/menu-collections/` | GET | 获取用户菜单权限集合 |
| 设置用户菜单权限集合 | `/api/v1/users/{id}/menu-collections/` | POST | 设置用户菜单权限集合 |
| 用户功能权限集合 | `/api/v1/users/{id}/permission-collections/` | GET | 获取用户功能权限集合 |
| 设置用户功能权限集合 | `/api/v1/users/{id}/permission-collections/` | POST | 设置用户功能权限集合 |
| 用户资源权限 | `/api/v1/users/{id}/resource-scopes/` | GET | 获取用户资源权限 |
| 授权用户资源 | `/api/v1/users/{id}/resource-scopes/batch/` | POST | 批量授权用户资源 |
| 撤销用户资源 | `/api/v1/users/{id}/resource-scopes/{scope_id}/` | DELETE | 撤销用户资源权限 |
| 部门列表 | `/api/v1/departments/` | GET | 获取部门列表 |
| 新增部门 | `/api/v1/departments/` | POST | 新增部门 |
| 更新部门 | `/api/v1/departments/{id}/` | PUT | 更新部门信息 |
| 删除部门 | `/api/v1/departments/{id}/` | DELETE | 删除部门 |
| 职务列表 | `/api/v1/positions/` | GET | 获取职务列表 |
| 新增职务 | `/api/v1/positions/` | POST | 新增职务 |
| 更新职务 | `/api/v1/positions/{id}/` | PUT | 更新职务信息 |
| 删除职务 | `/api/v1/positions/{id}/` | DELETE | 删除职务 |

---

**文档结束**

> **下一步**：完成模块八后，建议继续阅读 **模块九：Resources（资源管理）**，了解如何管理 Campaign Label、Unified Account Vault 和 Audiences Hub。
