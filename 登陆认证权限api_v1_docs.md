# SaaS Admin API 接口文档

# 一、文档基础信息

|项目|内容|
|---|---|
|文档标题|SaaS Admin API 接口文档|
|文档版本|v1.0.0|
|最后更新时间|2026-03-31 16:00:00|
|开发环境域名|*|
|测试环境域名|*|
|生产环境域名|*|
|全局请求头|1. Content-Type: application/json<br>2. Authorization: Bearer {token}（需登录接口必传）|
|全局错误码说明|0：成功<br>400：参数错误<br>401：未登录/Token过期<br>403：无权限<br>404：资源不存在<br>422：请求参数验证失败<br>409：资源已存在/冲突<br>500：服务器内部异常<br>详见附录业务错误码说明|

# 二、接口分组说明

本接口文档按业务模块进行分组，主要包括：
1. **核心权限 (Core)**：认证、用户、部门、职务、菜单、权限。
2. **集合管理 (Collections)**：菜单集合、权限集合。
3. **资源管理 (Resources)**：资源类型、资源类型规则、资源范围、资源范围明细。
4. **用户关系 (User Relations)**：用户与部门、职务、菜单、权限、资源范围及协作的关联。
5. **其他关联 (Other Relations)**：权限集合与权限、资源协作与范围的关联。

# 三、接口详细说明

## 1.1 认证模块 (Auth)

### 接口1：用户登录

|项目|内容|
|---|---|
|接口路径|/api/v1/auth/login|
|请求方法|POST|
|权限说明|无需登录|
|是否幂等|否|
|接口描述|用户通过账号密码登录，获取 Access Token 和 Refresh Token|

#### 1. 请求参数

##### 1.1 Body参数 (JSON)

|字段名|类型|是否必填|描述|示例值|
|---|---|---|---|---|
|username|string|是|用户名|admin|
|password|string|是|密码|123456|

#### 2. 响应结构

##### 2.1 响应字段说明

|字段名|类型|描述|示例值|
|---|---|---|---|
|code|int|状态码，0表示成功|0|
|message|string|提示信息|Login successful|
|data.access_token|string|访问令牌|eyJhbGci...|
|data.refresh_token|string|刷新令牌|eyJhbGci...|
|data.token_type|string|令牌类型|bearer|
|data.expires_in|int|过期时间(秒)|1800|

#### 3. 请求&响应示例

##### 3.1 请求示例 (curl)

```bash
curl --location --request POST 'http://localhost:8000/api/v1/auth/login' \
--header 'Content-Type: application/json' \
--data-raw '{
  "username": "admin",
  "password": "password123"
}'
```

##### 3.2 成功响应示例

```json
{
  "code": 0,
  "message": "Login successful",
  "data": {
    "access_token": "eyJhbGci...",
    "refresh_token": "eyJhbGci...",
    "token_type": "bearer",
    "expires_in": 1800
  },
  "meta": ""
}
```

---

### 接口2：用户登出

|项目|内容|
|---|---|
|接口路径|/api/v1/auth/logout|
|请求方法|POST|
|权限说明|需登录|
|是否幂等|是|
|接口描述|用户登出，销毁当前会话|

---

### 接口3：刷新令牌

|项目|内容|
|---|---|
|接口路径|/api/v1/auth/refresh|
|请求方法|POST|
|权限说明|无需登录|
|是否幂等|否|
|接口描述|使用刷新令牌获取新的访问令牌|

#### 1. 请求参数

##### 1.1 Body参数 (JSON)

|字段名|类型|是否必填|描述|示例值|
|---|---|---|---|---|
|refresh_token|string|是|刷新令牌|eyJhbGci...|

---


## 1.2 用户模块 (User)

### 接口1：创建用户

|项目|内容|
|---|---|
|接口路径|/api/v1/users|
|请求方法|POST|
|权限说明|需具备 `sys:user:create` 权限|
|频率限制|无|
|是否幂等|否|
|接口描述|创建一个新的系统用户。|

#### 1. 请求参数

##### 1.1 Body 参数 (JSON 格式)

|字段名|类型|是否必填|描述|示例值|枚举值|
|---|---|---|---|---|---|
|name|string|是|用户名|zhangsan|无|
|password|string|是|密码（6-50位）|123456|无|
|real_name|string|否|真实姓名|张三|无|
|email|string|否|邮箱|zhangsan@example.com|无|
|mobile|string|否|联系电话|13800138000|无|
|status|int|否|状态|1|1=Active, 2=InActive|
|is_superuser|int|否|是否超级用户|0|0=普通, 1=超级|

#### 2. 响应结构

##### 2.1 响应字段说明

|字段名|类型|描述|示例值|
|---|---|---|---|
|code|int|状态码，0表示成功|0|
|message|string|提示信息|User created successfully|
|data|object|用户信息|{...}|
|data.name|string|用户名|zhangsan|
|data.real_name|string|真实姓名|张三|
|data.email|string|邮箱|zhangsan@example.com|
|data.mobile|string|联系电话|13800138000|
|data.status|int|状态码|1|
|data.status_name|string|状态名称|Active|
|data.is_superuser|int|是否超级用户|0|
|data.is_superuser_name|string|是否超级用户名称|普通用户|
|data.last_login_at|string|最后登录时间|2023-10-27 10:00:00|
|data.created_at|string|创建时间|2023-10-27 10:00:00|
|data.updated_at|string|更新时间|2023-10-27 10:00:00|
|meta|string/array/object|详细信息/元数据|""|

#### 3. 请求&响应示例

##### 3.1 请求示例 (curl)

```bash
curl --location --request POST 'http://localhost:8000/api/v1/users' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{
  "name": "zhangsan",
  "password": "password123",
  "real_name": "张三",
  "email": "zhangsan@example.com",
  "mobile": "13800138000",
  "status": 1,
  "is_superuser": 0
}'
```

##### 3.2 成功响应示例

```json
{
  "code": 0,
  "message": "User created successfully",
  "data": {
    "name": "zhangsan",
    "real_name": "张三",
    "email": "zhangsan@example.com",
    "mobile": "13800138000",
    "status": 1,
    "status_name": "Active",
    "is_superuser": 0,
    "is_superuser_name": "普通用户",
    "last_login_at": null,
    "created_at": "2023-10-27 10:00:00",
    "updated_at": "2023-10-27 10:00:00"
  },
  "meta": ""
}
```

---
## 1.3 部门模块 (Department)

### 接口1：创建部门

|项目|内容|
|---|---|
|接口路径|/api/v1/departments|
|请求方法|POST|
|权限说明|需登录，需权限 `sys:department:create`|
|频率限制|无|
|是否幂等|否|
|接口描述|创建一个新的部门信息。|

#### 1. 请求参数

##### 1.1 Body参数（JSON格式）

|字段名|类型|是否必填|描述|示例值|枚举值|
|---|---|---|---|---|---|
|name|string|是|部门名称|研发部|无|
|code|string|是|部门编码|RD001|无|
|status|int|否|状态|1|1=Active, 2=InActive|
|description|string|否|描述信息|研发部门负责产品开发|无|
|parent_id|int|否|上级部门ID|0|0表示顶级部门|
|leader_id|int|否|部门负责人ID|1|无|
|sort_flag|int|否|排序标志|1|无|

#### 2. 响应结构

##### 2.1 响应字段说明

|字段名|类型|描述|示例值|枚举值|
|---|---|---|---|---|
|code|int|状态码，0表示成功|0|参考全局错误码|
|message|string|提示信息|Department created successfully|无|
|data|object|创建的部门详情|{...}|无|
|data.name|string|部门名称|研发部|无|
|data.code|string|部门编码|RD001|无|
|data.status|int|状态|1|1=Active, 2=InActive|
|data.status_name|string|状态名称|Active|无|
|data.description|string|描述信息|研发部门负责产品开发|无|
|data.parent_id|int|上级部门ID|0|无|
|data.leader_id|int|部门负责人ID|1|无|
|data.sort_flag|int|排序标志|1|无|
|data.path|string|部门路径|0/1/|无|
|data.level|int|部门层级|1|无|

#### 3. 请求&响应示例

##### 3.1 请求示例（curl）

```bash
curl --location --request POST 'http://localhost:8000/api/v1/departments' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{
  "name": "研发部",
  "code": "RD001",
  "status": 1,
  "description": "研发部门负责产品开发",
  "parent_id": 0,
  "leader_id": 1,
  "sort_flag": 1
}'
```

##### 3.2 成功响应示例

```json
{
  "code": 0,
  "message": "Department created successfully",
  "data": {
    "name": "研发部",
    "code": "RD001",
    "status": 1,
    "status_name": "Active",
    "description": "研发部门负责产品开发",
    "parent_id": 0,
    "leader_id": 1,
    "sort_flag": 1,
    "path": "0/",
    "level": 1
  },
  "meta": ""
}
```

---

### 接口2：获取部门列表（分页）

|项目|内容|
|---|---|
|接口路径|/api/v1/departments|
|请求方法|GET|
|权限说明|需登录，需权限 `sys:department:list`|
|频率限制|无|
|是否幂等|是|
|接口描述|分页获取部门列表，支持按名称、编码、状态和父级ID过滤。|

#### 1. 请求参数

##### 1.1 Query参数

|字段名|类型|是否必填|描述|示例值|枚举值|
|---|---|---|---|---|---|
|name|string|否|部门名称（模糊搜索）|研发|无|
|code|string|否|部门编码（模糊搜索）|RD|无|
|status|int|否|状态|1|1=Active, 2=InActive|
|parent_id|int|否|上级部门ID|0|无|
|page|int|否|页码|1|默认1|
|page_size|int|否|每页条目数|10|默认10|

#### 2. 响应结构

##### 2.1 响应字段说明

|字段名|类型|描述|示例值|
|---|---|---|---|
|code|int|状态码|0|
|message|string|提示信息|Departments retrieved successfully|
|data|object|分页响应数据|{...}|
|data.total|int|总条目数|10|
|data.page|int|当前页码|1|
|data.page_size|int|每页条目数|10|
|data.items|array|部门列表数据|[]|

#### 3. 请求&响应示例

##### 3.1 请求示例（curl）

```bash
curl --location --request GET 'http://localhost:8000/api/v1/departments?name=研发&page=1&page_size=10' \
--header 'Authorization: Bearer {token}'
```

##### 3.2 成功响应示例

```json
{
  "code": 0,
  "message": "Departments retrieved successfully",
  "data": {
    "total": 1,
    "page": 1,
    "page_size": 10,
    "items": [
      {
        "name": "研发部",
        "code": "RD001",
        "status": 1,
        "status_name": "Active",
        "description": "研发部门",
        "parent_id": 0,
        "leader_id": 1,
        "sort_flag": 1,
        "path": "0/",
        "level": 1
      }
    ]
  },
  "meta": ""
}
```

---

### 接口3：获取部门树形结构

|项目|内容|
|---|---|
|接口路径|/api/v1/departments/tree|
|请求方法|GET|
|权限说明|需权限 `sys:department:list`|
|是否幂等|是|
|接口描述|获取部门的树形结构数据。|

#### 1. 请求参数

##### 1.1 Query参数

|字段名|类型|是否必填|描述|示例值|
|---|---|---|---|---|
|status|int|否|状态过滤|1|

#### 2. 响应结构

##### 2.1 响应字段说明

|字段名|类型|描述|示例值|
|---|---|---|---|
|data|array|树形节点列表|[]|
|data.children|array|子部门列表|[]|

#### 3. 请求&响应示例

##### 3.2 成功响应示例

```json
{
  "code": 0,
  "message": "Department tree retrieved successfully",
  "data": [
    {
      "name": "总公司",
      "code": "HQ",
      "status": 1,
      "status_name": "Active",
      "children": [
        {
          "name": "研发部",
          "code": "RD001",
          "status": 1,
          "status_name": "Active",
          "children": []
        }
      ]
    }
  ],
  "meta": ""
}
```

---

### 接口4：获取部门选项列表（下拉框）

|项目|内容|
|---|---|
|接口路径|/api/v1/departments/options|
|请求方法|GET|
|权限说明|需权限 `sys:department:list`|
|接口描述|获取用于下拉框的部门选项列表。|

#### 2. 响应结构字段说明

|字段名|类型|描述|
|---|---|---|
|data[].id|int|部门ID|
|data[].name|string|部门名称|
|data[].code|string|部门编码|
|data[].parent_id|int|上级部门ID|
|data[].status|int|状态|
|data[].status_name|string|状态名称|

---

### 接口5：获取部门详情

|项目|内容|
|---|---|
|接口路径|/api/v1/departments/{department_id}|
|请求方法|GET|
|权限说明|需权限 `sys:department:view`|

#### 1. 请求参数

##### 1.1 路径参数

|字段名|类型|是否必填|描述|示例值|
|---|---|---|---|---|
|department_id|int|是|部门ID|1|

---

### 接口6：更新部门信息

|项目|内容|
|---|---|
|接口路径|/api/v1/departments/{department_id}|
|请求方法|PUT|
|权限说明|需权限 `sys:department:update`|

#### 1. 请求参数

##### 1.2 Body参数（JSON格式）

|字段名|类型|是否必填|描述|
|---|---|---|---|
|name|string|否|部门名称|
|status|int|否|状态|
|description|string|否|描述信息|
|parent_id|int|否|上级部门ID|
|leader_id|int|否|负责人ID|
|sort_flag|int|否|排序标志|

---

### 接口7：删除部门

|项目|内容|
|---|---|
|接口路径|/api/v1/departments/{department_id}|
|请求方法|DELETE|
|权限说明|需权限 `sys:department:delete`|
|接口描述|软删除指定部门及其关联信息。|

---

### 接口8：更新部门状态

|项目|内容|
|---|---|
|接口路径|/api/v1/departments/{department_id}/status|
|请求方法|PATCH|
|权限说明|需权限 `sys:department:update`|

#### 1. Body参数

|字段名|类型|是否必填|描述|
|---|---|---|---|
|status|int|是|状态码（1-Active, 2-InActive）|

---

## 1.4 职务模块 (Position)

### 接口1：创建职务

|项目|内容|
|---|---|
|接口路径|/api/v1/positions|
|请求方法|POST|
|权限说明|需权限 `sys:position:create`|
|接口描述|创建职务记录。|

#### 1. 请求参数

##### 1.1 Body参数（JSON格式）

|字段名|类型|是否必填|描述|示例值|
|---|---|---|---|---|
|name|string|是|职务名称|高级开发工程师|
|code|string|是|职务编码|SR_DEV|
|status|int|否|状态|1|
|description|string|否|职务描述|无|
|sort_flag|int|否|排序标志|0|

#### 2. 响应结构字段说明

|字段名|类型|描述|
|---|---|---|
|data.name|string|职务名称|
|data.code|string|职务编码|
|data.status|int|状态码|
|data.status_name|string|状态名称|
|data.sort_flag|int|排序标志|

#### 3. 请求示例

```bash
curl --location --request POST 'http://localhost:8000/api/v1/positions' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{
  "name": "高级开发工程师",
  "code": "SR_DEV",
  "status": 1
}'
```

---

### 接口2：获取职务列表（分页）

|项目|内容|
|---|---|
|接口路径|/api/v1/positions|
|请求方法|GET|
|权限说明|需权限 `sys:position:list`|

#### 1. 请求参数

##### 1.1 Query参数

|字段名|类型|是否必填|描述|
|---|---|---|---|
|name|string|否|职务名称（模糊）|
|code|string|否|职务编码（模糊）|
|status|int|否|状态|
|page|int|否|页码|
|page_size|int|否|页大小|

---

### 接口3：获取职务选项列表（下拉框）

|项目|内容|
|---|---|
|接口路径|/api/v1/positions/options|
|请求方法|GET|
|权限说明|需权限 `sys:position:list`|

#### 2. 响应结构字段说明

|字段名|类型|描述|
|---|---|---|
|data[].id|int|职务ID|
|data[].name|string|职务名称|
|data[].code|string|职务编码|
|data[].status|int|状态|
|data[].status_name|string|状态名称|

---

### 接口4：获取职务详情

|项目|内容|
|---|---|
|接口路径|/api/v1/positions/{position_id}|
|请求方法|GET|
|权限说明|需权限 `sys:position:view`|

---

### 接口5：更新职务信息

|项目|内容|
|---|---|
|接口路径|/api/v1/positions/{position_id}|
|请求方法|PUT|
|权限说明|需权限 `sys:position:update`|

#### 1. Body参数

|字段名|类型|是否必填|描述|
|---|---|---|---|
|name|string|否|职务名称|
|status|int|否|状态|
|description|string|否|描述|
|sort_flag|int|否|排序标志|

---

### 接口6：删除职务

|项目|内容|
|---|---|
|接口路径|/api/v1/positions/{position_id}|
|请求方法|DELETE|
|权限说明|需权限 `sys:position:delete`|

---

### 接口7：更新职务状态

|项目|内容|
|---|---|
|接口路径|/api/v1/positions/{position_id}/status|
|请求方法|PATCH|
|权限说明|需权限 `sys:position:update`|

#### 1. Body参数

|字段名|类型|是否必填|描述|
|---|---|---|---|
|status|int|是|状态码|

---

## 1.5 菜单模块 (Menu)

### 接口1：创建菜单

|项目|内容|
|---|---|
|接口路径|/api/v1/menus|
|请求方法|POST|
|权限说明|需登录，需权限：sys:menu:create|
|频率限制|无|
|是否幂等|否|
|接口描述|创建新的系统菜单、目录或按钮。|

#### 1. 请求参数

##### 1.1 Body参数（JSON格式）

|字段名|类型|是否必填|描述|示例值|枚举值（可选）|
|---|---|---|---|---|---|
|parent_id|int|是|父菜单ID，0表示顶级菜单|0|无|
|name|string|是|菜单名称（唯一标识）|UserManagement|无|
|title|string|是|菜单前端显示名称|用户管理|无|
|path|string|否|菜单前端路径|/system/user|无|
|icon|string|否|菜单图标|user|无|
|target|int|否|打开方式|1|1=当前窗口, 2=新窗口|
|menu_type|int|否|菜单类型|1|0=目录, 1=菜单, 2=按钮|
|sort_flag|int|否|排序标志|10|无|
|status|int|否|状态|1|1=Active, 2=InActive|

#### 2. 响应结构

##### 2.1 响应字段说明

|字段名|类型|描述|示例值|
|---|---|---|---|
|code|int|状态码，0表示成功|0|
|msg|string|提示信息|Menu created successfully|
|data|object|菜单详细信息|{...}|
|data.id|int|菜单ID|1|
|data.parent_id|int|父菜单ID|0|
|data.name|string|菜单名称|UserManagement|
|data.title|string|显示名称|用户管理|
|data.path|string|前端路径|/system/user|
|data.icon|string|图标|user|
|data.target|int|打开方式值|1|
|data.target_name|string|打开方式名称|当前窗口|
|data.menu_type|int|菜单类型值|1|
|data.menu_type_name|string|菜单类型名称|菜单|
|data.sort_flag|int|排序标志|10|
|data.status|int|状态值|1|
|data.status_name|string|状态名称|Active|
|data.level|int|层级|1|

#### 3. 请求&响应示例

##### 3.1 请求示例（curl）

```bash
curl --location --request POST 'http://localhost:8000/api/v1/menus' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{
  "parent_id": 0,
  "name": "SystemConfig",
  "title": "系统配置",
  "path": "/system",
  "icon": "setting",
  "menu_type": 0,
  "sort_flag": 1
}'
```

---

### 接口2：获取菜单树形结构

|项目|内容|
|---|---|
|接口路径|/api/v1/menus/tree|
|请求方法|GET|
|权限说明|需登录，需权限：sys:menu:list|
|频率限制|无|
|是否幂等|是|
|接口描述|获取所有菜单并以树形结构返回，支持状态和类型过滤。|

#### 1. 请求参数

##### 1.1 Query参数

|字段名|类型|是否必填|描述|示例值|枚举值（可选）|
|---|---|---|---|---|---|
|status|int|否|状态过滤|1|1=Active, 2=InActive|
|menu_type|int|否|菜单类型过滤|1|0=目录, 1=菜单, 2=按钮|

#### 2. 响应结构

##### 2.1 响应字段说明

|字段名|类型|描述|示例值|
|---|---|---|---|
|data|array|菜单树节点列表|[]|
|data[].children|array|子菜单列表|[]|

---

### 接口3：获取菜单选项列表

|项目|内容|
|---|---|
|接口路径|/api/v1/menus/options|
|请求方法|GET|
|权限说明|需登录，需权限：sys:menu:list|
|频率限制|无|
|是否幂等|是|
|接口描述|获取菜单选项列表（用于下拉框），包含菜单类型信息。|

---

### 接口4：获取菜单列表（分页）

|项目|内容|
|---|---|
|接口路径|/api/v1/menus|
|请求方法|GET|
|权限说明|需登录，需权限：sys:menu:list|
|频率限制|无|
|是否幂等|是|
|接口描述|分页获取菜单列表，支持多条件模糊搜索。|

---

## 1.6 权限模块 (Permission)

### 接口1：创建权限

|项目|内容|
|---|---|
|接口路径|/api/v1/permissions|
|请求方法|POST|
|权限说明|需登录，需权限：sys:permission:create|
|频率限制|无|
|是否幂等|否|
|接口描述|创建功能权限（如：按钮操作权限、API 访问权限）。|

#### 1. 请求参数

##### 1.1 Body参数（JSON格式）

|字段名|类型|是否必填|描述|示例值|
|---|---|---|---|---|
|name|string|是|权限名称|用户创建|
|code|string|是|权限编码（用于代码权限校验）|sys:user:create|
|api_path|string|否|API 访问路径|/api/v1/users|
|method|string|否|请求方法|POST|
|status|int|否|状态|1|

#### 2. 响应结构

##### 2.1 响应字段说明

|字段名|类型|描述|示例值|
|---|---|---|---|
|data.id|int|权限ID|1|
|data.name|string|权限名称|用户创建|
|data.code|string|权限编码|sys:user:create|
|data.api_path|string|API路径|/api/v1/users|
|data.method|string|请求方法|POST|
|data.status|int|状态|1|

#### 3. 请求&响应示例

##### 3.1 请求示例（curl）

```bash
curl --location --request POST 'http://localhost:8000/api/v1/permissions' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{\n  "name": "菜单创建",\n  "code": "sys:menu:create",\n  "api_path": "/api/v1/menus",\n  "method": "POST"\n}'
```

---

### 接口2：获取权限列表（分页）

|项目|内容|
|---|---|
|接口路径|/api/v1/permissions|
|请求方法|GET|
|权限说明|需登录，需权限：sys:permission:list|
|频率限制|无|
|是否幂等|是|
|接口描述|分页获取权限列表，支持按名称、编码和状态查询。|

---

# 2. 集合管理 (Collections)

## 模块名称：菜单集合 (Menu Collection)

### 接口 1：创建菜单集合

|项目|内容|
|---|---|
|接口路径|/api/v1/menu-collections|
|请求方法|POST|
|权限说明|需登录，需权限 `sys:menu-collection:create`|
|频率限制|无|
|是否幂等|否|
|接口描述|创建新的菜单集合，用于对菜单进行逻辑分组。|

#### 1. 请求参数

##### 1.3 Body 参数（JSON 格式）

|字段名|类型|是否必填|描述|示例值|枚举值|
|---|---|---|---|---|---|
|name|string|是|菜单集合名称|运营管理系统|无|
|status|int|否|状态|1|1=Active, 2=InActive|
|description|string|否|描述信息|运营系统的菜单集合|无|

#### 2. 响应结构

##### 2.1 响应字段说明

|字段名|类型|描述|示例值|
|---|---|---|---|
|code|int|状态码，0 表示成功|0|
|msg|string|提示信息|success|
|data|object|菜单集合信息|{}|
|data.id|int|菜单集合 ID|1|
|data.name|string|菜单集合名称|运营管理系统|
|data.status|int|状态|1|
|data.status_name|string|状态名称|Active|
|data.description|string|描述信息|运营系统的菜单集合|
|data.menu_count|int|包含菜单数量|0|

#### 3. 请求 & 响应示例

##### 3.1 请求示例（curl）

```bash
curl --location --request POST 'http://localhost:8000/api/v1/menu-collections' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{
  "name": "运营管理系统",
  "status": 1,
  "description": "运营系统的菜单集合"
}'
```

##### 3.2 成功响应示例

```json
{
  "code": 0,
  "message": "Menu collection created successfully",
  "data": {
    "id": 1,
    "name": "运营管理系统",
    "status": 1,
    "status_name": "Active",
    "description": "运营系统的菜单集合",
    "menu_count": 0,
    "created_at": "2023-10-27 10:00:00",
    "updated_at": "2023-10-27 10:00:00"
  }
}
```

---

### 接口 2：获取菜单集合列表（分页）

|项目|内容|
|---|---|
|接口路径|/api/v1/menu-collections|
|请求方法|GET|
|权限说明|需登录，需权限 `sys:menu-collection:list`|
|频率限制|无|
|是否幂等|是|
|接口描述|分页获取菜单集合列表，支持按名称模糊搜索和状态过滤。|

---

## 模块名称：权限集合 (Permission Collection)

### 接口 1：创建权限集合

|项目|内容|
|---|---|
|接口路径|/api/v1/permission-collections|
|请求方法|POST|
|权限说明|需登录，需权限 `sys:permission-collection:create`|
|频率限制|无|
|是否幂等|否|
|接口描述|创建权限集合，用于对细粒度权限点进行逻辑打包。|

---

# 3. 资源管理 (Resources)

## 模块名称：资源类型 (Resource Type)

### 接口 1：获取资源类型列表（分页）

|项目|内容|
|---|---|
|接口路径|/api/v1/resource-types|
|请求方法|GET|
|权限说明|需登录，需权限 `sys:resource-type:list`|
|频率限制|无|
|是否幂等|是|
|接口描述|分页获取系统定义的资源类型（如：应用、广告主、渠道等）。|

---

## 模块名称：资源类型规则 (Resource Type Rule)

### 接口 1：创建资源类型规则

|项目|内容|
|---|---|
|接口路径|/api/v1/resource-type-rules|
|请求方法|POST|
|权限说明|需登录，需权限 `sys:resource-type-rule:create`|
|频率限制|无|
|是否幂等|否|
|接口描述|定义资源类型对应的底层数据库表及过滤字段，用于数据权限自动化过滤。|

---

## 模块名称：资源范围 (Resource Scope)

### 接口 1：创建资源范围

|项目|内容|
|---|---|
|接口路径|/api/v1/resource-scopes|
|请求方法|POST|
|权限说明|需登录，需权限 `sys:resource-scope:create`|
|频率限制|无|
|是否幂等|否|
|接口描述|创建资源范围，作为资源的逻辑容器（如：某个项目的资源组）。|

---

## 模块名称：资源范围明细 (Resource Scope Detail)

### 接口 1：批量创建资源范围明细

|项目|内容|
|---|---|
|接口路径|/api/v1/resource-scope-details/batch|
|请求方法|POST|
|权限说明|需登录，需权限 `sys:resource-scope-detail:create`|
|频率限制|无|
|是否幂等|否|
|接口描述|批量向资源范围中添加具体的资源 ID 及访问权限。|

---

## 4. 用户关系 (User Relations)

### 4.1 User-Department (用户-部门关系)
*   **主要接口**:
    *   `POST /api/v1/user-departments` (创建关系)
    *   `GET /api/v1/user-departments` (查询关系列表)
    *   `POST /api/v1/user-departments/batch` (批量创建)
    *   `DELETE /api/v1/user-departments` (删除关系)
    *   `GET /api/v1/user-departments/users/{user_id}/departments` (查询用户所属部门)
    *   `GET /api/v1/user-departments/departments/{department_id}/users` (查询部门下的用户)
*   **典型接口详情**:
    *   **创建关系**: `POST /api/v1/user-departments`
        *   参数: `{ "user_id": 1, "department_id": 1 }`
        *   响应: `ApiResponse[UserDepartmentResponse]`
    *   **查询关系**: `GET /api/v1/user-departments`
        *   参数: `user_id`, `department_id`, `page`, `page_size`
        *   响应: `ApiResponse[PaginatedResponse[UserDepartmentResponse]]`
*   **其他接口**:
    *   `POST /api/v1/user-departments/users/departments` (为用户设置部门)
    *   `POST /api/v1/user-departments/departments/users` (为部门设置用户)
    *   `PATCH /api/v1/user-departments/departments/{department_id}/leader` (设置部门负责人)

### 4.2 User-Position (用户-职务关系)
*   **主要接口**:
    *   `POST /api/v1/user-positions` (创建关系)
    *   `GET /api/v1/user-positions` (查询关系列表)
    *   `POST /api/v1/user-positions/batch` (批量创建)
    *   `DELETE /api/v1/user-positions` (删除关系)
*   **典型接口详情**:
    *   **创建关系**: `POST /api/v1/user-positions`
        *   参数: `{ "user_id": 1, "position_id": 1 }`
        *   响应: `ApiResponse[UserPositionResponse]`
    *   **查询关系**: `GET /api/v1/user-positions`
        *   参数: `user_id`, `position_id`, `page`, `page_size`
        *   响应: `ApiResponse[PaginatedResponse[UserPositionResponse]]`

### 4.3 User-Menu (用户-菜单关系)
*   **主要接口**:
    *   `POST /api/v1/user-menus` (创建关系)
    *   `GET /api/v1/user-menus` (查询关系列表)
    *   `POST /api/v1/user-menus/batch` (批量创建)
    *   `DELETE /api/v1/user-menus` (删除关系)
*   **典型接口详情**:
    *   **创建关系**: `POST /api/v1/user-menus`
        *   参数: `{ "user_id": 1, "menu_id": 1 }`
        *   响应: `ApiResponse[UserMenuResponse]`

### 4.4 User-Menu-Coll (用户-菜单集合关系)
*   **主要接口**:
    *   `POST /api/v1/user-menu-collections` (创建关系)
    *   `GET /api/v1/user-menu-collections` (查询关系列表)
*   **典型接口详情**:
    *   **创建关系**: `POST /api/v1/user-menu-collections`
        *   参数: `{ "user_id": 1, "menu_collection_id": 1 }`
        *   响应: `ApiResponse[UserMenuCollectionResponse]`

### 4.5 User-Permission (用户-权限关系)
*   **主要接口**:
    *   `POST /api/v1/user-permissions` (创建关系)
    *   `GET /api/v1/user-permissions` (查询关系列表)

### 4.6 User-Perm-Coll (用户-权限集合关系)
*   **主要接口**:
    *   `POST /api/v1/user-permission-collections` (创建关系)
    *   `GET /api/v1/user-permission-collections` (查询关系列表)

### 4.7 User-Resource-Scope (用户-资源范围关系)
*   **主要接口**:
    *   `POST /api/v1/user-resource-scopes` (创建关系)
    *   `GET /api/v1/user-resource-scopes` (查询关系列表)

### 4.8 User-Resource-Collab (用户-资源协作关系)
*   **主要接口**:
    *   `POST /api/v1/user-resource-collaborations` (创建协作)
    *   `GET /api/v1/user-resource-collaborations` (查询协作列表)
    *   `PUT /api/v1/user-resource-collaborations/{collaboration_id}` (更新协作)
    *   `GET /api/v1/user-resource-collaborations/{collaboration_id}` (获取单个协作)
    *   `DELETE /api/v1/user-resource-collaborations/{collaboration_id}` (删除协作)
*   **典型接口详情**:
    *   **创建关系**: `POST /api/v1/user-resource-collaborations`
        *   参数: `{ "granter_id": 1, "grantee_id": 2, "access_type": 1 }` (access_type: 0-跟随data_scope, 1-只读, 2-读写)
        *   响应: `ApiResponse[UserResourceCollaborationResponse]`

---

## 5. 其他关联关系 (Other Relations)

### 5.1 Perm-Coll-Perm (权限集合-权限关系)
*   **主要接口**:
    *   `POST /api/v1/permission-collection-permissions` (创建关系)
    *   `GET /api/v1/permission-collection-permissions` (查询关系列表)

### 5.2 Res-Collab-Scope (资源协作-范围关系)
*   **主要接口**:
    *   `POST /api/v1/resource-collaboration-scopes` (创建关系)
    *   `GET /api/v1/resource-collaboration-scopes` (查询关系列表)

---

# 四、附录

## 1. 业务错误码详情

|错误码|HTTP状态|错误描述|说明|
|---|---|---|---|
|0|200|success|操作成功|
|400|400|Bad Request|请求结构错误|
|401|401|Unauthorized|未认证|
|403|403|Forbidden|权限不足|
|404|404|Not Found|资源不存在|
|422|422|Validation Error|数据校验失败|
|409|409|Conflict|资源冲突(已存在)|
|901001|401|AUTH_NOT_LOGGED_IN|未登录|
|901004|401|AUTH_EXPIRED|认证已过期|
|1001|400|LOGIN_ACCOUNT_PASSWORD_ERROR|用户名或密码错误|
|2001|404|USER_NOT_FOUND|用户不存在|
|2100|409|USER_EXISTS|用户已存在|
|3001|404|DEPARTMENT_NOT_FOUND|部门不存在|
|3100|409|DEPARTMENT_EXISTS|部门编码已存在|
|3300|400|DEPARTMENT_HAS_CHILDREN|存在子部门，无法删除|
|4001|404|POSITION_NOT_FOUND|职务不存在|
|5001|404|MENU_NOT_FOUND|菜单不存在|
|10001|404|PERMISSION_NOT_FOUND|权限点不存在|

## 2. 工具说明

1. 接口文档采用 OpenAPI 规范，开发者可通过 `/docs` 查看交互式文档。
2. 响应体外层结构统一为：`{"code": 0, "message": "...", "data": {...}, "meta": "..."}`。
3. 分页查询统一使用 `page` 和 `page_size` 参数。
