# 广告主管理 API 文档

> 生成时间: 2026-03-20
> API 版本: v1
> Base URL: `/api/v1/advertisers`

---

## 目录

1. [概述](#概述)
2. [认证](#认证)
3. [接口列表](#接口列表)
4. [接口详情](#接口详情)
5. [数据结构](#数据结构)
6. [错误码](#错误码)
7. [使用示例](#使用示例)

---

## 概述

广告主管理 API 提供完整的 CRUD 操作，支持：

- 广告主的创建、查询、更新、删除
- 多条件过滤和搜索
- 分页查询
- 按 BD/AM 用户查询

### 基本信息

| 项目 | 说明 |
|------|------|
| 认证方式 | Bearer Token (JWT) |
| 权限控制 | RBAC (基于角色) |
| 数据格式 | JSON |
| 字符编码 | UTF-8 |

---

## 认证

所有接口都需要认证，请在请求 Header 中添加：

```
Authorization: Bearer <your_token>
```

获取 Token：

```bash
POST /api/v1/auth/login
Content-Type: application/json

{
  "username": "your_username",
  "password": "your_password"
}
```

---

## 接口列表

| 方法 | 路径 | 功能 | 权限代码 |
|------|------|------|---------|
| GET | `/advertisers` | 查询广告主列表 | advertiser:view |
| GET | `/advertisers/{advertiser_id}` | 查询广告主详情 | advertiser:detail |
| POST | `/advertisers` | 创建广告主 | advertiser:create |
| PUT | `/advertisers/{advertiser_id}` | 更新广告主 | advertiser:update |
| DELETE | `/advertisers/{advertiser_id}` | 删除广告主 | advertiser:delete |
| PATCH | `/advertisers/{advertiser_id}/status` | 更新广告主状态 | advertiser:update_status |
| GET | `/advertisers/bd/{bd_user_id}` | 查询 BD 的广告主 | advertiser:view_by_bd |
| GET | `/advertisers/am/{am_user_id}` | 查询 AM 的广告主 | advertiser:view_by_am |

---

## 接口详情

### 1. 查询广告主列表

**GET** `/api/v1/advertisers`

查询广告主列表，支持分页和多条件过滤。

**请求参数 (Query)**

| 参数 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| page | integer | 否 | 1 | 页码，从 1 开始 |
| page_size | integer | 否 | 10 | 每页数量，1-100 |
| status | integer | 否 | - | 状态过滤：1-active, 2-Stop |
| bd | integer | 否 | - | BD 用户 ID 过滤 |
| am | integer | 否 | - | AM 用户 ID 过滤 |
| name | string | 否 | - | 搜索关键词（搜索 name, user_name, email, company_name）|
| advertiser_id | integer | 否 | - | 广告主 ID 精确过滤 |

**响应示例**

```json
{
  "code": 0,
  "message": "success",
  "data": {
    "total": 100,
    "advertisers": [
      {
        "id": 1,
        "name": "Example Advertiser",
        "user_name": "example_user",
        "email": "adv@example.com",
        "notification_email": "notify@example.com",
        "bd": 1,
        "am": 2,
        "status": 1,
        "company_name": "Example Inc.",
        "payment_terms": 7,
        "payment_type": 1,
        "margin": 0.15,
        "jointime": "2026-01-01T00:00:00",
        "update_time": "2026-03-20T10:30:00"
      }
    ]
  }
}
```

**curl 示例**

```bash
# 基本查询
curl -X GET "http://localhost:8000/api/v1/advertisers" \
  -H "Authorization: Bearer $TOKEN"

# 分页查询
curl -X GET "http://localhost:8000/api/v1/advertisers?page=1&page_size=20" \
  -H "Authorization: Bearer $TOKEN"

# 状态过滤
curl -X GET "http://localhost:8000/api/v1/advertisers?status=1" \
  -H "Authorization: Bearer $TOKEN"

# 搜索关键词
curl -X GET "http://localhost:8000/api/v1/advertisers?name=test" \
  -H "Authorization: Bearer $TOKEN"

# 组合查询
curl -X GET "http://localhost:8000/api/v1/advertisers?status=1&bd=1&page=1&page_size=10" \
  -H "Authorization: Bearer $TOKEN"
```

---

### 2. 查询广告主详情

**GET** `/api/v1/advertisers/{advertiser_id}`

根据 ID 查询单个广告主详情。

**路径参数**

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| advertiser_id | integer | 是 | 广告主 ID |

**响应示例**

```json
{
  "code": 0,
  "message": "success",
  "data": {
    "id": 1,
    "name": "Example Advertiser",
    "user_name": "example_user",
    "email": "adv@example.com",
    "notification_email": "notify@example.com",
    "bd": 1,
    "am": 2,
    "status": 1,
    "memo": "Important client",
    "ip_whitelist": "192.168.1.1,192.168.1.2",
    "click_caps": "1000",
    "conversion_caps": "100",
    "offer_permission": "premium",
    "margin": 0.15,
    "contract_id": "CONTRACT-001",
    "contract_valid_to": "2026-12-31",
    "company_name": "Example Inc.",
    "company_nick_name": "Example",
    "payment_terms": 7,
    "payment_type": 1,
    "benificial_name": "John Doe",
    "bank_name": "Bank of America",
    "swfit_code": "BOFAUS3N",
    "address": "123 Main St, City, Country",
    "campaign_do_type": 1,
    "block_channel": "",
    "campaign_click_caps": 0,
    "weget_control": 0.0,
    "weget_max_control": 0.0,
    "package_block": "",
    "campaign_id_filter": "",
    "white_channels": "",
    "campaign_update_time": 0,
    "cam_channel_conv_limit": 0,
    "cam_conv_limit": 0,
    "is_testing": 0,
    "short_clickid": 0,
    "auto_test": 0,
    "is_more_tracking": 0,
    "api": "",
    "postback_url": "",
    "reject_cap_must": 0,
    "geo_filter": "",
    "jointime": "2026-01-01T00:00:00",
    "update_time": "2026-03-20T10:30:00"
  }
}
```

**curl 示例**

```bash
curl -X GET "http://localhost:8000/api/v1/advertisers/1" \
  -H "Authorization: Bearer $TOKEN"
```

---

### 3. 创建广告主

**POST** `/api/v1/advertisers`

创建新广告主。

**请求体 (JSON)**

| 字段 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| name | string | 否 | - | 广告主名称，最大 256 字符 |
| user_name | string | 否 | - | 广告主用户名，最大 256 字符 |
| email | string | 否 | - | 邮箱，最大 256 字符 |
| notification_email | string | 否 | - | 通知邮箱，最大 256 字符 |
| bd | integer | 否 | - | BD 用户 ID |
| am | integer | 否 | - | AM 用户 ID |
| status | integer | 否 | 1 | 状态：1-active, 2-Stop |
| memo | string | 否 | - | 备注信息，最大 256 字符 |
| ip_whitelist | string | 否 | - | IP 白名单，多个用英文逗号分隔，最大 1024 字符 |
| click_caps | string | 否 | - | 点击 cap，最大 512 字符 |
| conversion_caps | string | 否 | - | 转换 cap，最大 512 字符 |
| offer_permission | string | 否 | - | offer 权限，最大 512 字符 |
| margin | float | 否 | - | 利润率，范围 [0, 3] |
| contract_id | string | 否 | - | 合同 ID，最大 512 字符 |
| contract_valid_to | string | 否 | - | 合同有效期，最大 512 字符 |
| company_name | string | 否 | - | 公司名称，最大 512 字符 |
| company_nick_name | string | 否 | - | 公司昵称，最大 256 字符 |
| payment_terms | integer | 否 | 7 | 付款条款：7, 15, 30, 45, 60, 90 |
| payment_type | integer | 否 | 1 | 付款类型：1-Invoice needed, 2-Invoice free |
| benificial_name | string | 否 | - | 受益人名称，最大 256 字符 |
| bank_name | string | 否 | - | 银行名称，最大 256 字符 |
| swfit_code | string | 否 | - | SWIFT 代码，最大 256 字符 |
| address | string | 否 | - | 地址，最大 256 字符 |
| campaign_do_type | integer | 否 | 1 | Campaign 处理类型：1-Auto, 2-Manual |
| block_channel | string | 否 | "" | 屏蔽渠道，最大 2560 字符 |
| campaign_click_caps | integer | 否 | 0 | 每个 Campaign 每日的点击上限 |
| weget_control | float | 否 | 0.0 | 最小价格 |
| weget_max_control | float | 否 | 0.0 | 最大价格 |
| package_block | string | 否 | "" | 包屏蔽，最大 1024 字符 |
| campaign_id_filter | string | 否 | - | Campaign ID 过滤，最大 8192 字符 |
| white_channels | string | 否 | "" | 白名单渠道，最大 2048 字符 |
| cam_channel_conv_limit | integer | 否 | 0 | 渠道转化限制 |
| cam_conv_limit | integer | 否 | 0 | 转化限制 |
| is_testing | integer | 否 | 0 | 是否测试：0-No, 1-Yes |
| short_clickid | integer | 否 | 0 | 短点击 ID：0-No, 1-Yes |
| auto_test | integer | 否 | 0 | 自动测试：0-No, 1-Yes |
| is_more_tracking | integer | 否 | 0 | 更多追踪：0-No, 1-Yes |
| api | string | 否 | - | API |
| postback_url | string | 否 | - | Postback URL |
| reject_cap_must | integer | 否 | 0 | 拒绝 cap 必须：0-No, 1-Yes |
| geo_filter | string | 否 | "" | 地理位置过滤，最大 256 字符 |

**请求示例**

```json
{
  "name": "New Advertiser",
  "user_name": "new_adv",
  "email": "new@example.com",
  "notification_email": "notify@example.com",
  "bd": 1,
  "am": 2,
  "status": 1,
  "memo": "New client",
  "company_name": "New Company Inc.",
  "company_nick_name": "NewCo",
  "payment_terms": 7,
  "payment_type": 1,
  "margin": 0.15,
  "benificial_name": "Jane Doe",
  "bank_name": "Citibank",
  "swfit_code": "CITIUS33"
}
```

**响应示例**

```json
{
  "code": 0,
  "message": "success",
  "data": {
    "id": 100,
    "name": "New Advertiser",
    "user_name": "new_adv",
    "email": "new@example.com",
    ...
  }
}
```

**curl 示例**

```bash
curl -X POST "http://localhost:8000/api/v1/advertisers" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "New Advertiser",
    "email": "new@example.com",
    "bd": 1,
    "am": 2,
    "company_name": "New Company Inc."
  }'
```

---

### 4. 更新广告主

**PUT** `/api/v1/advertisers/{advertiser_id}`

更新广告主信息，所有字段可选。

**路径参数**

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| advertiser_id | integer | 是 | 广告主 ID |

**请求体 (JSON)**

所有字段同创建接口，全部为可选字段。

**响应示例**

```json
{
  "code": 0,
  "message": "success",
  "data": {
    "id": 1,
    "name": "Updated Name",
    "email": "updated@example.com",
    ...
  }
}
```

**curl 示例**

```bash
curl -X PUT "http://localhost:8000/api/v1/advertisers/1" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Updated Name",
    "margin": 0.20
  }'
```

---

### 5. 删除广告主

**DELETE** `/api/v1/advertisers/{advertiser_id}`

删除广告主。

**路径参数**

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| advertiser_id | integer | 是 | 广告主 ID |

**响应示例**

```json
{
  "code": 0,
  "message": "Advertiser deleted successfully",
  "data": null
}
```

**curl 示例**

```bash
curl -X DELETE "http://localhost:8000/api/v1/advertisers/1" \
  -H "Authorization: Bearer $TOKEN"
```

---

### 6. 更新广告主状态

**PATCH** `/api/v1/advertisers/{advertiser_id}/status`

快速更新广告主状态。

**路径参数**

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| advertiser_id | integer | 是 | 广告主 ID |

**请求参数 (Query)**

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| status | integer | 是 | 状态：1-active, 2-Stop |

**响应示例**

```json
{
  "code": 0,
  "message": "success",
  "data": {
    "id": 1,
    "status": 2,
    ...
  }
}
```

**curl 示例**

```bash
# 停止广告主
curl -X PATCH "http://localhost:8000/api/v1/advertisers/1/status?status=2" \
  -H "Authorization: Bearer $TOKEN"

# 激活广告主
curl -X PATCH "http://localhost:8000/api/v1/advertisers/1/status?status=1" \
  -H "Authorization: Bearer $TOKEN"
```

---

### 7. 查询 BD 的广告主

**GET** `/api/v1/advertisers/bd/{bd_user_id}`

查询指定 BD 用户负责的所有广告主。

**路径参数**

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| bd_user_id | integer | 是 | BD 用户 ID |

**响应示例**

```json
{
  "code": 0,
  "message": "success",
  "data": [
    {
      "id": 1,
      "name": "Advertiser 1",
      "bd": 1,
      ...
    },
    {
      "id": 2,
      "name": "Advertiser 2",
      "bd": 1,
      ...
    }
  ]
}
```

**curl 示例**

```bash
curl -X GET "http://localhost:8000/api/v1/advertisers/bd/1" \
  -H "Authorization: Bearer $TOKEN"
```

---

### 8. 查询 AM 的广告主

**GET** `/api/v1/advertisers/am/{am_user_id}`

查询指定 AM 用户负责的所有广告主。

**路径参数**

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| am_user_id | integer | 是 | AM 用户 ID |

**响应示例**

```json
{
  "code": 0,
  "message": "success",
  "data": [
    {
      "id": 1,
      "name": "Advertiser 1",
      "am": 2,
      ...
    }
  ]
}
```

**curl 示例**

```bash
curl -X GET "http://localhost:8000/api/v1/advertisers/am/2" \
  -H "Authorization: Bearer $TOKEN"
```

---

## 数据结构

### AdvertiserResponse

广告主完整信息结构。

```json
{
  "id": 1,
  "name": "Example Advertiser",
  "user_name": "example_user",
  "email": "adv@example.com",
  "notification_email": "notify@example.com",
  "bd": 1,
  "am": 2,
  "status": 1,
  "memo": "Important client",
  "ip_whitelist": "192.168.1.1,192.168.1.2",
  "click_caps": "1000",
  "conversion_caps": "100",
  "offer_permission": "premium",
  "margin": 0.15,
  "contract_id": "CONTRACT-001",
  "contract_valid_to": "2026-12-31",
  "company_name": "Example Inc.",
  "company_nick_name": "Example",
  "payment_terms": 7,
  "payment_type": 1,
  "benificial_name": "John Doe",
  "bank_name": "Bank of America",
  "swfit_code": "BOFAUS3N",
  "address": "123 Main St, City, Country",
  "campaign_do_type": 1,
  "block_channel": "",
  "campaign_click_caps": 0,
  "weget_control": 0.0,
  "weget_max_control": 0.0,
  "package_block": "",
  "campaign_id_filter": "",
  "white_channels": "",
  "campaign_update_time": 0,
  "cam_channel_conv_limit": 0,
  "cam_conv_limit": 0,
  "is_testing": 0,
  "short_clickid": 0,
  "auto_test": 0,
  "is_more_tracking": 0,
  "api": "",
  "postback_url": "",
  "reject_cap_must": 0,
  "geo_filter": "",
  "jointime": "2026-01-01T00:00:00",
  "update_time": "2026-03-20T10:30:00"
}
```

### AdvertiserListResponse

广告主列表响应结构。

```json
{
  "total": 100,
  "advertisers": [/* AdvertiserResponse array */]
}
```

### 枚举值说明

| 字段 | 可选值 | 说明 |
|------|--------|------|
| status | 1, 2 | 1-active(活跃), 2-Stop(停止) |
| payment_terms | 7, 15, 30, 45, 60, 90 | 付款期限（天）|
| payment_type | 1, 2 | 1-Invoice needed(需要发票), 2-Invoice free(无需发票) |
| campaign_do_type | 1, 2 | 1-Auto(自动), 2-Manual(手动) |
| is_testing | 0, 1 | 0-No, 1-Yes |
| short_clickid | 0, 1 | 0-No, 1-Yes |
| auto_test | 0, 1 | 0-No, 1-Yes |
| is_more_tracking | 0, 1 | 0-No, 1-Yes |
| reject_cap_must | 0, 1 | 0-No, 1-Yes |

---

## 错误码

| 错误码 | 说明 |
|--------|------|
| 0 | 成功 |
| 40001 | 请求参数验证错误 |
| 40002 | 资源不存在 |
| 40003 | 资源已存在 |
| 40004 | 业务错误 |
| 40005 | 权限不足 |
| 401 | 未认证 |
| 403 | 禁止访问 |
| 500 | 服务器内部错误 |

### 错误响应格式

```json
{
  "code": 40001,
  "message": "Request validation failed",
  "data": [
    {
      "field": "payment_terms",
      "input": 5,
      "message": "Input should be one of [7, 15, 30, 45, 60, 90]"
    }
  ]
}
```

### 常见错误

**1. 权限不足**

```json
{
  "code": 40005,
  "message": "Permission denied: advertiser:create",
  "data": null
}
```

**2. 资源不存在**

```json
{
  "code": 40002,
  "message": "Advertiser not found with id: 999",
  "data": null
}
```

**3. 验证错误**

```json
{
  "code": 40001,
  "message": "Request validation failed",
  "data": [
    {
      "field": "margin",
      "input": 5.0,
      "message": "Input should be less than or equal to 3"
    }
  ]
}
```

---

## 使用示例

### 完整工作流

```bash
#!/bin/bash

# 1. 登录获取 token
TOKEN=$(curl -s -X POST "http://localhost:8000/api/v1/auth/login" \
  -H "Content-Type: application/json" \
  -d '{"username": "admin", "password": "your_password"}' \
  | jq -r '.data.access_token')

# 2. 创建广告主
curl -X POST "http://localhost:8000/api/v1/advertisers" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Test Advertiser",
    "email": "test@example.com",
    "bd": 1,
    "am": 1,
    "company_name": "Test Company"
  }'

# 3. 查询广告主列表
curl -X GET "http://localhost:8000/api/v1/advertisers?page=1&page_size=10" \
  -H "Authorization: Bearer $TOKEN"

# 4. 更新广告主
curl -X PUT "http://localhost:8000/api/v1/advertisers/1" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"margin": 0.20}'

# 5. 停止广告主
curl -X PATCH "http://localhost:8000/api/v1/advertisers/1/status?status=2" \
  -H "Authorization: Bearer $TOKEN"

# 6. 删除广告主
curl -X DELETE "http://localhost:8000/api/v1/advertisers/1" \
  -H "Authorization: Bearer $TOKEN"
```

### Python 示例

```python
import requests

BASE_URL = "http://localhost:8000"
TOKEN = "your_token_here"
HEADERS = {
    "Authorization": f"Bearer {TOKEN}",
    "Content-Type": "application/json"
}

# 创建广告主
data = {
    "name": "Test Advertiser",
    "email": "test@example.com",
    "bd": 1,
    "am": 1,
    "company_name": "Test Company"
}
response = requests.post(
    f"{BASE_URL}/api/v1/advertisers",
    headers=HEADERS,
    json=data
)
print(response.json())

# 查询列表
response = requests.get(
    f"{BASE_URL}/api/v1/advertisers",
    headers=HEADERS,
    params={"page": 1, "page_size": 10, "status": 1}
)
print(response.json())
```

---

## 相关文档

- [API 使用文档](./advertiser_api_usage.md)
- [快速开始指南](./advertiser_quick_start.md)
- [实现总结](./advertiser_implementation_summary.md)
- [权限管理文档](./permission_usage.md)
- [操作日志文档](./operate_log_body_fix.md)
- [数据库表结构](../db/advertiser.sql)
