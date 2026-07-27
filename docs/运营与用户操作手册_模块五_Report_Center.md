# 运营与用户操作手册 · 模块五  
## Report Center 数据报表与分析操作 SOP

> **适用角色**：商务（BD）、渠道经理（PM）、财务、管理层  
> **对应系统页面**：`Report Center`（/report）  
> **文档版本**：v1.0 · 2026-06-22  

---

## 1. 业务场景与心智

Report Center 是平台的**全维度数据分析中枢**，覆盖从原始流量到财务结算的全链路数据。

| 业务场景 | 说明 |
|---------|------|
| 日常业绩监控 | 查看 Imp/Clicks/Conv/Revenue/Payout/Profit 等核心指标 |
| 多维度下钻分析 | 按 Advertiser/Publisher/Campaign/Country/OS/Hour 等维度分组查看 |
| 环比对比分析 | 对比今天 vs 昨天、本周 vs 上周的数据变化 |
| 异常流量识别 | 通过 Rej.Count、CR%、Ecpc 等指标识别低质流量 |
| 市场行情洞察 | 查看市场均价趋势，发现增收机会 |
| 预算来源追踪 | 追踪广告主的预算消耗来源和去向 |

> ⚠️ **核心概念**：Report Center 的数据是**只读**的，不支持直接编辑。所有数据来自后端 API（`/api/v1/report/...`）。

---

## 2. 系统入口与界面概览

### 2.1 进入方式

1. 鼠标点击左侧主导航栏 **Report Center**（📊 图标）
2. 系统默认进入 **「Internal Performance Hub」** Tab

### 2.2 界面布局

Report Center 包含 **4 个一级 Tab**，分别对应不同的分析场景：

| Tab | 图标 | 标题 | 副标题 | 功能状态 |
|-----|------|------|--------|---------|
| `performance` | 📈 TrendingUp | Internal Performance Hub | 全维度业绩分析与多维决策看板 | ✅ 已上线 |
| `quality` | 🔍 Search | Quality & Insight Center | 后置行为深度挖掘与流量质量审计 | ⚠️ 功能开发中 |
| `intelligence` | 🌐 Globe | Market Intelligence | 全球市场行情预测与增收机会洞察 | ✅ 部分上线 |
| `labs` | 🧪 FlaskConical | Special Labs | 预算来源追踪与系统运行诊断 | ✅ 部分上线 |

---

## 3. Internal Performance Hub（全维度业绩中心）

### 3.1 视图模式切换

Internal Performance Hub 支持 **3 种视图模式**，通过右上角的按钮切换：

| 模式 | 图标 | 说明 | 适用场景 |
|------|------|------|---------|
| **Standard** | 📊 LayoutGrid | 全维度报表，支持多维度和多指标 | 日常业绩监控、多维度下钻分析 |
| **Comparison** | 🔄 GitCompare | 对比模式，选择对比日期 | 环比分析、A/B 测试对比 |
| **Ranking** | 🏆 Trophy | Top 排行，查看 Top N 维度 | 快速发现优质/劣质 Advertiser、Publisher、Campaign |

#### 切换操作：

1. 在 Internal Performance Hub 页面的右上角，找到 **视图模式按钮组**
2. 点击目标模式图标（Standard / Comparison / Ranking）
3. 页面切换到对应视图

---

### 3.2 维度分组（Group By）

维度分组决定数据的**聚合粒度**。

#### 可选维度（9 个）：

| 维度 Key | 标签 | 图标 | 说明 |
|----------|------|------|------|
| `date` | Date | 📅 Calendar | 按日期聚合 |
| `hour` | Hour | 🕐 Clock | 按小时聚合 |
| `advertiser` | Advertiser | 🏢 Building2 | 按广告主聚合 |
| `publisher` | Publisher | 👥 Users | 按渠道商聚合 |
| `campaign` | Campaign | 🎯 Target | 按广告活动聚合 |
| `packageName` | Package | 📦 Package | 按应用包名聚合 |
| `country` | Country | 📍 MapPin | 按国家聚合 |
| `os` | OS | 📱 Smartphone | 按操作系统聚合（iOS/Android）|
| `channel` | Channel | 👤 User | 按渠道聚合 |
| `TcampaignId` | T-CamID | #️⃣ Hash | 按第三方广告活动 ID 聚合 |

#### 维度组合规则：

- **最多选 2 个维度**（双维度组合）
- **维度白名单**（合法的双维度组合）：

| 主维度 | 可组合的副维度 |
|---------|----------------|
| `campaign` | `country`、`publisher`、`hour` |
| `publisher` | `campaign`、`country`、`hour` |
| `hour` | `campaign`、`publisher`、`country` |
| `country` | `campaign`、`publisher`、`hour` |
| `packageName` | ❌ 不支持双维度 |
| `TcampaignId` | ❌ 不支持双维度 |
| `advertiser` / `channel` | ❌ 只能单选 |

> 💡 **技巧**：想查看「美国地区的 Top Campaign」，先选 `country`，再选 `campaign`（或反过来）。

#### 操作步骤：

1. 在页面上方的 **「Group By」** 区域，点击维度标签
2. 第一次点击：选中该维度作为**主维度**
3. 第二次点击（可选）：选中该维度作为**副维度**
4. 点击已选中的标签：取消选中
5. 维度选择完成后，点击 **「Search」** 按钮，系统按新维度重新聚合数据

---

### 3.3 指标选择（Metrics）

指标决定表格中显示的**数据列**。

#### 指标分类（3 类）：

##### ① Value Metrics（数值指标）

| 指标 Key | 标签 | 格式化 | 说明 |
|----------|------|--------|------|
| `impressions` | Imp. | number | 展示量 |
| `clicks` | Clicks | number | 点击量 |
| `conversions` | Conv. | number | 转化量 |
| `postbackConv` | Pb Conv. | number | Postback 转化量 |
| `revenue` | Revenue | currency | 营收（广告主单价 × 转化量）|
| `payout` | Payout | currency | 成本（渠道单价 × 转化量）|
| `profit` | Profit | currency | 毛利（Revenue - Payout）|
| `averagePrice` | Avg. Prc. | currency | 平均单价 |

##### ② Ratio Metrics（比率指标）

| 指标 Key | 标签 | 格式化 | 说明 |
|----------|------|--------|------|
| `margin` | Margin% | percent | 毛利率（Profit / Revenue × 100%）|
| `cr` | CR% | percent | 转化率（Conversions / Clicks × 100%）|

##### ③ Specialized Metrics（专项指标）

| 指标 Key | 标签 | 格式化 | 说明 |
|----------|------|--------|------|
| `rejectCount` | Rej. Count | number | 拒绝转化量（反欺诈过滤）|
| `ecpc` | Ecpc | currency | 每点击收益（Revenue / Clicks）|

#### 操作步骤：

1. 点击页面右上角的 **「Settings」** 按钮（⚙️ 图标）
2. 系统弹出 **Report Settings Panel**（右侧抽屉）
3. 在面板中，展开 **「Value Metrics」** / **「Ratio Metrics」** / **「Specialized Metrics」** 分组
4. 勾选需要显示的指标（支持多选）
5. 点击 **「Apply」** 按钮，系统保存设置并刷新表格

> 💡 **技巧**：常用的指标组合可以保存为**预设模板**（见 3.5 节）。

---

### 3.4 全局筛选器（Global Filter Bar）

全局筛选器用于**过滤数据源**，只查询符合条件的数据。

#### 筛选字段（11 个）：

| 筛选字段 | 系统字段名 | 组件类型 | 说明 |
|----------|------------|---------|------|
| Date Range | `dateRange` | DateRangePickerInline | 日期范围（支持快捷预设）|
| Advertiser | `advertiser` | MultiSelect | 广告主多选 |
| Publisher | `publisher` | MultiSelect | 渠道商多选 |
| Campaign | `campaign` | FilterInput | 广告活动 ID/名称搜索 |
| Package | `packageName` | FilterInput | 应用包名搜索 |
| Country | `country` | MultiSelect | 国家多选 |
| OS | `os` | Checkbox Group | iOS / Android |
| AM | `am` | MultiSelect | 账户经理多选 |
| PM | `pm` | MultiSelect | 渠道经理多选 |
| Hour | `hour` | MultiSelect | 小时多选（0-23）|

#### 日期快捷预设（9 个）：

| 预设 | 说明 |
|------|------|
| 今天 | 当天 |
| 昨天 | 前一天 |
| 最近三天 | 前天 + 昨天 + 今天 |
| 最近七天 | 过去 7 天（含今天）|
| 最近三十天 | 过去 30 天（含今天）|
| 本周 | 本周一至今 |
| 上周 | 上周一至上周日 |
| 本月 | 本月 1 日至今 |
| 上月 | 上月整月 |

#### 操作步骤：

1. 在页面上方的 **「Global Filter Bar」** 区域，找到目标筛选字段
2. 对该字段进行筛选操作：
   - **Date Range**：点击日期输入框，选择开始/结束日期（或点击快捷预设）
   - **MultiSelect**（Advertiser/Publisher/Country/AM/PM/Hour）：点击下拉框，勾选目标选项，点击空白处关闭
   - **FilterInput**（Campaign/Package）：输入关键词，点击「Search」按钮
   - **Checkbox Group**（OS）：点击 iOS/Android 标签切换选中状态
3. 所有筛选字段设置完成后，点击 **「Search」** 按钮（🔍 图标）
4. 系统根据筛选条件重新请求 API，刷新表格数据

> ⚠️ **注意**：筛选器是**延迟生效**的（点击 Search 才触发），不是实时生效。

---

### 3.5 预设模板（Preset Templates）

预设模板是**常用的维度和指标组合**，一键加载，避免重复配置。

#### 系统内置模板（3 个）：

| 模板 ID | 名称 | 描述 | 维度 | 指标 |
|---------|------|------|------|------|
| `daily-profit-loss` | Daily Profit & Loss | 每日盈亏看板 | Date + Advertiser + Publisher | Clicks, Conv., Pb Conv., Revenue, Payout, Profit, Margin%, CR% |
| `quality-audit` | Quality & Anti-Fraud Audit | 质量与反欺诈审计 | Publisher + Campaign + Country | Imp., Clicks, Conv., Pb Conv., CR%, Rej. Count, Ecpc |
| `strategy-optimization` | Strategy & Optimization | 策略与优化 | Package + Campaign + OS | Payout, Revenue, Profit, Avg. Prc., Ecpc |

#### 操作步骤：

1. 点击页面右上角的 **「Settings」** 按钮（⚙️ 图标）
2. 系统弹出 **Report Settings Panel**（右侧抽屉）
3. 在面板中，找到 **「Preset Templates」** 分组
4. 点击目标模板卡片
5. 系统自动加载该模板的维度和指标配置
6. 点击 **「Apply」** 按钮，系统保存设置并刷新表格

> 💡 **技巧**：也可以创建**自定义模板**（点击「Save as Template」），保存当前维度和指标配置。

---

### 3.6 分析筛选器（Analysis Filter Bar）

分析筛选器用于**在已聚合的数据上做二次过滤**，类似 SQL 的 `HAVING` 子句。

#### 筛选字段（6 个）：

| 字段 Key | 标签 | 说明 |
|----------|------|------|
| `profit` | Profit | 毛利 |
| `margin` | Margin% | 毛利率 |
| `revenue` | Revenue | 营收 |
| `conversions` | Conversions | 转化量 |
| `cr` | CR% | 转化率 |
| `ecpc` | Ecpc | 每点击收益 |

#### 运算符（6 个）：

| 运算符 | 标签 | 说明 |
|--------|------|------|
| `gt` | Greater than | 大于 |
| `gte` | Greater than or equal | 大于等于 |
| `lt` | Less than | 小于 |
| `lte` | Less than or equal | 小于等于 |
| `eq` | Equal | 等于 |
| `neq` | Not equal | 不等于 |

#### 作用域（2 个）：

| 作用域 | 标签 | 说明 |
|--------|------|------|
| `having` | Having | 对聚合后的结果过滤（默认）|
| `where` | Where | 对原始数据过滤 |

#### 操作步骤：

1. 在 **Global Filter Bar** 下方，找到 **「Analysis」** 筛选器
2. 点击 **「+ Add Filter」** 按钮
3. 在弹出的行内，选择：
   - **字段**（如 Profit）
   - **运算符**（如 Greater than）
   - **阈值**（如 `100`）
   - **作用域**（如 Having）
4. 点击 **「Apply」** 按钮
5. 系统重新请求 API，只返回符合条件的数据

> 💡 **示例**：想查看「毛利率 > 30% 的 Campaign」，在 Analysis Filter 中设置：`margin` `gt` `30` `having`。

---

### 3.7 数据脱敏（Masking）

数据脱敏用于**隐藏敏感数据**（如具体金额），只显示趋势。

#### 操作步骤：

1. 在页面上方的 **「Global Filter Bar」** 区域，找到 **「Mask」** 开关
2. 点击开关，切换脱敏状态：
   - **关闭**（默认）：显示真实数据
   - **开启**：金额类指标显示为 `****`（隐藏具体数值）

> ⚠️ **适用场景**：截图分享、投屏演示时开启脱敏，防止敏感数据泄露。

---

### 3.8 对比模式（Comparison Mode）

对比模式用于**环比分析**，选择对比日期，系统并排展示两期数据。

#### 操作步骤：

1. 切换视图模式到 **「Comparison」**（点击右上角的 🔄 图标）
2. 在页面上方的 **日期选择器**中，设置：
   - **主日期**（Date）：当前分析日期
   - **对比日期**（Compare Date）：环比日期
3. 点击 **「Search」** 按钮
4. 系统并排展示两期数据，并计算变化率（Change%）

> 💡 **示例**：想查看「今天 vs 昨天」的对比，设置 Date = 今天，Compare Date = 昨天。

---

### 3.9 排行模式（Ranking Mode）

排行模式用于**快速发现 Top/Nottom N**，支持按指标降序/升序排行。

#### 操作步骤：

1. 切换视图模式到 **「Ranking」**（点击右上角的 🏆 图标）
2. 在页面上方的 **「Top N」** 输入框中，输入排行数量（如 `10`）
3. 点击 **「Search」** 按钮
4. 系统展示 Top N 排行表格，并按当前排序指标高亮

> 💡 **技巧**：点击表格列的**排序箭头**，切换升序/降序排行。

---

### 3.10 导出（Export）

导出功能用于将当前报表数据下载到本地。

#### 操作步骤：

1. 在表格右上角，找到 **「Download」** 按钮（⬇️ 图标）
2. 点击按钮
3. 系统生成 Excel 文件并自动下载

> ⚠️ **待确认**：导出的具体字段清单需要实际查看系统界面后补充。

---

## 4. Quality & Insight Center（质量与洞察中心）

### 4.1 业务场景与心智

Quality & Insight Center 是**后置行为深度挖掘与流量质量审计**的专用模块，用于识别低质流量、发现欺诈行为。

### 4.2 当前状态

> ⚠️ **功能开发中**：当前系统显示占位符页面，功能尚未上线。

**占位符页面内容**：
- 图标：🔍（Search）
- 标题：Quality & Insight Center
- 副标题：后置行为深度挖掘与流量质量审计 · 功能开发中

### 4.3 预计功能（基于代码推断）

根据代码中的 `QUALITY_AUDIT` 预设模板，预计上线后包含以下功能：

| 功能 | 说明 |
|------|------|
| 质量审计报表 | 按 Publisher/Campaign/Country 维度查看质量指标 |
| 反欺诈分析 | 通过 Rej. Count、CR%、Ecpc 等指标识别欺诈流量 |
| 流量质量排行 | 找出质量最差的 Publisher/Campaign |
| 异常预警 | 当 CR% 异常高/低时触发通知 |

> ⚠️ **待确认**：该功能的上线时间和具体实现需要向开发团队确认。

---

## 5. Market Intelligence（市场情报中心）

### 5.1 业务场景与心智

Market Intelligence 是**全球市场行情预测与增收机会洞察**的专用模块，用于查看市场均价趋势、发现增收机会。

### 5.2 子 Tab 说明

Market Intelligence 包含 **2 个子 Tab**：

| 子 Tab | 说明 | 功能状态 |
|--------|------|---------|
| **Market Offer** | 市场行情明细 | ✅ 已上线（Mock 数据）|
| **Budget Sources** | 预算来源追踪 | ✅ 已上线 |

---

### 5.3 Market Offer（市场行情）

Market Offer 展示**全球市场的应用单价行情**，帮助商务人员了解市场均价、发现增收机会。

#### 表格字段（11 个）：

| 字段 | 说明 | 是否可筛选 |
|------|------|-----------|
| Package Name | 应用包名 | ✅ |
| Country | 国家 | ✅ |
| Market Price | 市场均价 | ❌ |
| Internal Price | 内部单价 | ❌ |
| Network | 广告网络 | ❌ |
| Publisher ID | 渠道商 ID | ✅ |
| Update Date | 更新日期 | ✅ |
| Marketing Potential | 营销潜力（1-5 星）| ❌ |
| Frequency | 更新频率 | ❌ |
| Market Volume | 市场成交量 | ❌ |

#### 价格分布趋势图：

页面上方展示**价格分布趋势图**（折线图），包含以下曲线：
- **Market Avg**：市场均价
- **Internal Avg**：内部均价
- **P25**：25% 分位数
- **P75**：75% 分位数

#### 操作步骤：

1. 进入 **Report Center → Market Intelligence**
2. 系统默认进入 **「Market Offer」** 子 Tab
3. 在页面上方的**筛选器**区域，设置筛选条件：
   - **Package Name**：输入应用包名关键词
   - **Country**：选择国家
   - **Payout Mode**：选择计费模式（CPA/CPI/CPS/CPM/CPC）
   - **Price Range**：设置单价范围（Min/Max）
   - **Publisher ID**：选择渠道商
4. 点击 **「Search」** 按钮
5. 系统刷新表格和趋势图

---

### 5.4 Budget Sources（预算来源）

Budget Sources 展示**广告主的预算消耗来源和去向**，帮助商务人员了解预算分配。

> ⚠️ **待确认**：该 Tab 的具体字段和操作步骤需要实际查看系统界面后补充。

---

## 6. Special Labs（特殊实验室）

### 6.1 业务场景与心智

Special Labs 是**预算来源追踪与系统运行诊断**的专用模块，包含 XDJ Report 和 Traffic Hub (DMP) 两个子功能。

### 6.2 子 Tab 说明

Special Labs 包含 **2 个子 Tab**：

| 子 Tab | 说明 | 功能状态 |
|--------|------|---------|
| **XDJ Report** | XDJ 报告 | ✅ 已上线 |
| **Traffic Hub (DMP)** | 流量中心（数据管理平台）| ✅ 已上线 |

---

### 6.3 XDJ Report（XDJ 报告）

XDJ Report 是**自定义报告**功能，支持按 Advertiser/Publisher 筛选，生成定制化报告。

> ⚠️ **待确认**：该 Tab 的具体字段和操作步骤需要实际查看系统界面后补充。

---

### 6.4 Traffic Hub (DMP)（流量中心）

Traffic Hub 是**数据管理平台（DMP）**，用于管理流量标签、受众定向。

> ⚠️ **待确认**：该 Tab 的具体字段和操作步骤需要实际查看系统界面后补充。

---

## 7. 常见问题（FAQ）

### Q1：为什么我选了 2 个维度，系统提示"该组合不支持"？

**A**：维度组合受白名单限制。请参考 **3.2 节** 的维度组合规则，选择合法的双维度组合。

---

### Q2：为什么我设置了筛选条件，表格数据没有变化？

**A**：全局筛选器是**延迟生效**的，需要点击 **「Search」** 按钮才会触发查询。请检查是否忘记点击 Search。

---

### Q3：为什么 Comparison 模式下，变化率显示 "--"？

**A**：变化率需要**同时有主日期和对比日期的数据**才能计算。请检查对比日期是否选对，以及对比日期是否有数据。

---

### Q4：为什么 Quality & Insight Center 显示"功能开发中"？

**A**：该功能尚未上线，预计后续版本迭代。如需使用，请联系开发团队确认上线时间。

---

### Q5：如何保存常用的维度和指标配置？

**A**：使用 **预设模板** 功能（见 3.5 节）。也可以点击 Report Settings Panel 中的 **「Save as Template」** 按钮，保存当前配置。

---

## 附录：快捷操作索引

| 操作 | 入口 | 快捷键/说明 |
|------|------|-------------|
| 快速切换 Tab | Report Center 页面顶部 | 点击 Tab 标签 |
| 快速切换视图模式 | Internal Performance Hub 右上角 | 点击 Standard / Comparison / Ranking 图标 |
| 快速选择日期预设 | Date Range 选择器 | 点击快捷预设按钮（如"最近七天"）|
| 快速保存报表配置 | Report Settings Panel | 点击「Save as Template」|
| 快速导出报表 | 表格右上角 | 点击「Download」按钮 |
| 快速开启数据脱敏 | Global Filter Bar | 点击「Mask」开关 |

---

*文档基于 `ReportCenter.jsx`（6057 行）的真实代码编写，如有功能迭代请以系统实际界面为准。*

*最后更新：2026-06-22*
