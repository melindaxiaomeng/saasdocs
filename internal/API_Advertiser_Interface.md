# Partner Hub - 广告主页面接口文档

## 接口概览

本文档定义了 Partner Hub 广告主管理页面的后端接口规范。

---

## 1. 广告主列表接口

### 1.1 获取广告主列表

**接口地址**: `GET /api/v1/advertisers`

**请求参数**:

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| keyword | string | 否 | 搜索关键词（名称/ID） |
| status | string | 否 | 状态筛选：active/inactive/pending |
| page | int | 否 | 页码，默认 1 |
| pageSize | int | 否 | 每页数量，默认 20 |

**响应结构**:

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "total": 100,
    "page": 1,
    "pageSize": 20,
    "list": [
      {
        "id": "ADV_APPX",
        "name": "AppX Studios",
        "country": "US",
        "status": "active",
        "activeCampaigns": 3,
        "todayRevenue": 4280,
        "monthRevenue": 89400,
        "am": "Alice Wang",
        "postbackUrl": "https://postback.appxstudios.com/cb?clickid={clickid}&status={status}",
        "api": "https://api.appxstudios.com/v1/offers",
        "email": "am@appxstudios.com",
        "getOfferType": "Auto",
        "wegetMin": 0.5,
        "wegetMax": 9999,
        "campUpdateTime": "2024-01-15 14:30:00"
      }
    ]
  }
}
```

---

## 2. 广告主详情接口

### 2.1 获取广告主详情

**接口地址**: `GET /api/v1/advertisers/{advertiserId}`

**响应结构**:

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "id": "ADV_APPX",
    "name": "AppX Studios",
    "country": "US",
    "status": "active",
    "activeCampaigns": 3,
    "todayRevenue": 4280,
    "monthRevenue": 89400,
    "am": "Alice Wang",
    "postbackUrl": "https://postback.appxstudios.com/cb?clickid={clickid}&status={status}",
    "api": "https://api.appxstudios.com/v1/offers",
    "email": "am@appxstudios.com",
    "getOfferType": "Auto",
    "wegetMin": 0.5,
    "wegetMax": 9999,
    "campUpdateTime": "2024-01-15 14:30:00",
    "businessDev": "张三",
    "advertiserManager": "Alice Wang",
    "notificationEmail": "notify@appxstudios.com",
    "memo": "Priority advertiser Q1"
  }
}
```

---

## 3. 广告主 CRUD 接口

### 3.1 创建广告主

**接口地址**: `POST /api/v1/advertisers`

**请求体**:

```json
{
  "name": "AppX Studios",
  "country": "US",
  "status": "active",
  "api": "https://api.appxstudios.com/v1/offers",
  "postbackUrl": "https://postback.appxstudios.com/cb?clickid={clickid}&status={status}",
  "email": "am@appxstudios.com",
  "notificationEmail": "notify@appxstudios.com",
  "businessDev": "张三",
  "advertiserManager": "Alice Wang",
  "getOfferType": "Auto",
  "wegetMin": 0.5,
  "wegetMax": 9999,
  "autoTest": true,
  "isMoreTracking": false,
  "rejectCapMust": false,
  "shortClickid": false,
  "whiteChannels": "CH001,CH002",
  "ipWhitelist": "192.168.1.1,10.0.0.1",
  "campaignIdFilter": "",
  "geoFilter": "US,UK,CA",
  "packageBlock": "com.block.app",
  "dailyConversionCaps": 1000,
  "dailyTotalAdvClickCaps": 10000,
  "margin": 15,
  "blockChannel": "",
  "contractId": "CTR-2024-001",
  "contractValidTo": "2024-12-31",
  "companyName": "AppX Studios Inc.",
  "companyNickName": "AppX",
  "paymentTerms": "Net 30",
  "paymentType": "Wire",
  "beneficialName": "John Doe",
  "bankName": "Chase Bank",
  "swiftCode": "CHASUS33",
  "address": "123 Main St, San Francisco, CA",
  "memo": "Priority advertiser"
}
```

### 3.2 更新广告主

**接口地址**: `PUT /api/v1/advertisers/{advertiserId}`

**请求体**: 同创建接口

### 3.3 删除广告主

**接口地址**: `DELETE /api/v1/advertisers/{advertiserId}`

---

## 4. Performance Matrix 接口

### 4.1 获取 Performance Matrix 数据

**接口地址**: `GET /api/v1/advertisers/{advertiserId}/performance`

**请求参数**:

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| startDate | string | 是 | 开始日期 (YYYY-MM-DD) |
| endDate | string | 是 | 结束日期 (YYYY-MM-DD) |

**响应结构**:

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "dates": ["03/10", "03/11", "03/12", "03/13", "03/14", "03/15", "03/16"],
    "summary": {
      "totalConv": 1248,
      "totalPB": 986,
      "totalClicks": 45200,
      "avgCR": 2.76
    },
    "campaigns": [
      {
        "id": "CMP001",
        "name": "Game Offer A",
        "status": "active",
        "weget": "$2.50",
        "cap": 1000,
        "dailyData": [
          { "date": "03/10", "conv": 45, "pb": 42, "clicks": 1200 },
          { "date": "03/11", "conv": 52, "pb": 48, "clicks": 1350 },
          { "date": "03/12", "conv": 38, "pb": 35, "clicks": 980 },
          { "date": "03/13", "conv": 61, "pb": 58, "clicks": 1580 },
          { "date": "03/14", "conv": 55, "pb": 52, "clicks": 1420 },
          { "date": "03/15", "conv": 48, "pb": 45, "clicks": 1250 },
          { "date": "03/16", "conv": 67, "pb": 64, "clicks": 1680 }
        ]
      }
    ]
  }
}
```

---

## 5. Campaign Routing 接口

### 5.1 获取 Campaign Routing 列表

**接口地址**: `GET /api/v1/advertisers/{advertiserId}/routing`

**响应结构**:

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "list": [
      {
        "id": 1,
        "publisherName": "Unity Ads",
        "publisherId": "PUB_3312",
        "type": "SDK",
        "campaignName": "Game Offer A",
        "campaignId": "CMP001",
        "margin": "13.5%",
        "fixPayout": "$16",
        "cap": 400,
        "postbackRate": "95%",
        "hourAllow": "00:00-23:59",
        "status": "active"
      }
    ]
  }
}
```

### 5.2 更新 Routing 配置

**接口地址**: `PUT /api/v1/advertisers/{advertiserId}/routing/{routingId}`

**请求体**:

```json
{
  "margin": "15.0%",
  "fixPayout": "$17",
  "cap": 500,
  "postbackRate": "96%",
  "hourAllow": "08:00-22:00"
}
```

### 5.3 删除 Routing 配置

**接口地址**: `DELETE /api/v1/advertisers/{advertiserId}/routing/{routingId}`

---

## 6. Publisher Global Rules 接口

### 6.1 获取 Publisher Global Rules 列表

**接口地址**: `GET /api/v1/advertisers/{advertiserId}/global-rules`

**响应结构**:

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "list": [
      {
        "id": 1,
        "publisherName": "Unity Ads",
        "publisherId": "PUB_3312",
        "margin": "13.5%",
        "cap": 400,
        "user": "Admin",
        "updateTime": "2024-01-15 14:30",
        "remarks": "Premium publisher",
        "traffic": {
          "cClickCap": "5000",
          "hourlyClickCap": "500",
          "perCamClickCap": "200",
          "perCamCap": "100",
          "autoBindCampaign": "Enabled"
        },
        "platform": {
          "whiteChannels": "CH001, CH002",
          "whiteChannelsType": "Premium",
          "os": "Android, iOS",
          "androidCap": "600",
          "iosCap": "400",
          "country": "US, UK, CA",
          "packageNameList": "com.app.game"
        },
        "risk": {
          "reCap": "800",
          "postbackRate": "95%",
          "filterSteps": "3",
          "minWeget1": "$0.50",
          "minWeget2": "$1.00"
        }
      }
    ]
  }
}
```

### 6.2 更新 Publisher Global Rule

**接口地址**: `PUT /api/v1/advertisers/{advertiserId}/global-rules/{ruleId}`

**请求体**:

```json
{
  "margin": "15.0%",
  "cap": 500,
  "remarks": "Updated remarks",
  "traffic": {
    "cClickCap": "6000",
    "hourlyClickCap": "600",
    "perCamClickCap": "250",
    "perCamCap": "120",
    "autoBindCampaign": "Disabled"
  },
  "platform": {
    "whiteChannels": "CH001, CH002, CH003",
    "whiteChannelsType": "Premium",
    "os": "Android, iOS",
    "androidCap": "700",
    "iosCap": "500",
    "country": "US, UK, CA, AU",
    "packageNameList": "com.app.game, com.app.finance"
  },
  "risk": {
    "reCap": "900",
    "postbackRate": "96%",
    "filterSteps": "4",
    "minWeget1": "$0.60",
    "minWeget2": "$1.20"
  }
}
```

### 6.3 删除 Publisher Global Rule

**接口地址**: `DELETE /api/v1/advertisers/{advertiserId}/global-rules/{ruleId}`

---

## 7. 数据模型定义

### 7.1 Advertiser 模型

```typescript
interface Advertiser {
  id: string;                    // 广告主ID
  name: string;                  // 名称
  country: string;               // 国家
  status: 'active' | 'inactive' | 'pending';  // 状态
  activeCampaigns: number;       // 活跃Campaign数
  todayRevenue: number;          // 今日收入
  monthRevenue: number;          // 本月收入
  am: string;                    // AM负责人
  postbackUrl: string;           // Postback回调地址
  api: string;                   // API地址
  email: string;                 // 邮箱
  getOfferType: 'Auto' | 'Manual';  // 获取Offer方式
  wegetMin: number;              // Weget最小值
  wegetMax: number;              // Weget最大值
  campUpdateTime: string;        // Campaign更新时间
  
  // 扩展字段（创建/编辑用）
  businessDev?: string;          // 商务拓展
  advertiserManager?: string;    // 广告主经理
  notificationEmail?: string;    // 通知邮箱
  autoTest?: boolean;            // 自动测试
  isMoreTracking?: boolean;      // 是否多追踪
  rejectCapMust?: boolean;       // Reject Cap是否必填
  shortClickid?: boolean;        // 短Clickid
  whiteChannels?: string;        // 白名单渠道
  ipWhitelist?: string;          // IP白名单
  campaignIdFilter?: string;     // Campaign ID过滤
  geoFilter?: string;            // 地域过滤
  packageBlock?: string;         // 包名黑名单
  dailyConversionCaps?: number;  // 日转化上限
  dailyTotalAdvClickCaps?: number;  // 日点击上限
  margin?: number;               // 利润率
  blockChannel?: string;         // 屏蔽渠道
  contractId?: string;           // 合同ID
  contractValidTo?: string;      // 合同有效期
  companyName?: string;          // 公司全称
  companyNickName?: string;      // 公司简称
  paymentTerms?: string;         // 付款条款
  paymentType?: string;          // 付款方式
  beneficialName?: string;       // 受益人姓名
  bankName?: string;             // 银行名称
  swiftCode?: string;            // SWIFT代码
  address?: string;              // 地址
  memo?: string;                 // 备注
}
```

### 7.2 CampaignRouting 模型

```typescript
interface CampaignRouting {
  id: number;
  publisherName: string;         // 渠道名称
  publisherId: string;           // 渠道ID
  type: string;                  // 类型 (SDK/Network/DSP等)
  campaignName: string;          // Campaign名称
  campaignId: string;            // Campaign ID
  margin: string;                // 利润率
  fixPayout: string;             // 固定出价
  cap: number;                   // 上限
  postbackRate: string;          // Postback率
  hourAllow: string;             // 允许时段
  status: 'active' | 'paused' | 'inactive';  // 状态
}
```

### 7.3 PublisherGlobalRule 模型

```typescript
interface PublisherGlobalRule {
  id: number;
  publisherName: string;         // 渠道名称
  publisherId: string;           // 渠道ID
  margin: string;                // 利润率
  cap: number;                   // 上限
  user: string;                  // 创建用户
  updateTime: string;            // 更新时间
  remarks: string;               // 备注
  traffic: TrafficConfig;        // 流量策略配置
  platform: PlatformConfig;      // 渠道与平台配置
  risk: RiskConfig;              // 风控与权重配置
}

interface TrafficConfig {
  cClickCap: string;             // C click CAP
  hourlyClickCap: string;        // 每小时点击上限
  perCamClickCap: string;        // 每Campaign点击上限
  perCamCap: string;             // 每Campaign上限
  autoBindCampaign: string;      // 自动绑定Campaign
}

interface PlatformConfig {
  whiteChannels: string;         // 白名单渠道
  whiteChannelsType: string;     // 白名单渠道类型
  os: string;                    // 操作系统
  androidCap: string;            // Android上限
  iosCap: string;                // iOS上限
  country: string;               // 国家
  packageNameList: string;       // 包名列表
}

interface RiskConfig {
  reCap: string;                 // Re Cap
  postbackRate: string;          // Postback率
  filterSteps: string;           // 过滤步骤
  minWeget1: string;             // 最小Weget1
  minWeget2: string;             // 最小Weget2
}
```

---

## 8. 错误码定义

| 错误码 | 说明 |
|--------|------|
| 200 | 成功 |
| 400 | 请求参数错误 |
| 401 | 未授权 |
| 403 | 权限不足 |
| 404 | 资源不存在 |
| 409 | 资源冲突 |
| 500 | 服务器内部错误 |

---

## 9. 接口调用示例

### 9.1 获取广告主列表

```bash
curl -X GET "https://api.example.com/api/v1/advertisers?keyword=AppX&page=1&pageSize=20" \
  -H "Authorization: Bearer {token}"
```

### 9.2 创建广告主

```bash
curl -X POST "https://api.example.com/api/v1/advertisers" \
  -H "Authorization: Bearer {token}" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "New Advertiser",
    "country": "US",
    "status": "active",
    "businessDev": "张三",
    "advertiserManager": "李四"
  }'
```

### 9.3 更新 Publisher Global Rule

```bash
curl -X PUT "https://api.example.com/api/v1/advertisers/ADV_APPX/global-rules/1" \
  -H "Authorization: Bearer {token}" \
  -H "Content-Type: application/json" \
  -d '{
    "margin": "15.0%",
    "cap": 500,
    "traffic": {
      "cClickCap": "6000"
    }
  }'
```

---

## 10. 页面功能对照

| 页面功能 | 对应接口 |
|----------|----------|
| 广告主列表 | GET /api/v1/advertisers |
| 创建广告主 | POST /api/v1/advertisers |
| 编辑广告主 | PUT /api/v1/advertisers/{id} |
| 删除广告主 | DELETE /api/v1/advertisers/{id} |
| Performance Matrix | GET /api/v1/advertisers/{id}/performance |
| Campaign Routing | GET /api/v1/advertisers/{id}/routing |
| 更新 Routing | PUT /api/v1/advertisers/{id}/routing/{routingId} |
| Publisher Global Rules | GET /api/v1/advertisers/{id}/global-rules |
| 更新 Global Rule | PUT /api/v1/advertisers/{id}/global-rules/{ruleId} |
| 删除 Global Rule | DELETE /api/v1/advertisers/{id}/global-rules/{ruleId} |
