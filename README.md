# Farfetch 价格追踪器

[![Bright Data](https://img.shields.io/badge/Powered%20by-Bright%20Data-blue?style=flat-square)](https://bright.cn)
[![Farfetch Price Tracker](https://img.shields.io/badge/Farfetch%20Price%20Tracker-Managed%20Solution-orange?style=flat-square)](https://bright.cn/products/insights/price-tracker/farfetch)
[![Python](https://img.shields.io/badge/Python-3.9%2B-yellow?style=flat-square)](https://python.org)

[![Bright Insights Price Tracker](https://raw.githubusercontent.com/danielshashko/bright-insights-assets/main/price-tracker-hero-v2.png)](https://bright.cn/products/insights/price-tracker/farfetch)

Farfetch 实时价格追踪——一个全球奢侈时尚在线零售平台。两种开始方式：**全托管**情报平台，或使用 Bright Data 的 AI Scraper Builder 构建的**自定义 scraper**。

---

## 选项 1：Bright Insights - AI 驱动的价格追踪（推荐）

**[Bright Insights](https://bright.cn/products/insights/price-tracker/farfetch)** 是 Bright Data 的全托管零售情报平台。无需构建 scraper，无需维护基础设施——只需将结构化、可直接用于分析的价格数据交付到仪表板、data feed 或你的 BI 工具中。

**为什么团队选择 Bright Insights：**
- 🚀 **零配置** - 使用开箱即用的仪表板和 data feed，几分钟内即可上线
- 🤖 **AI 驱动的推荐** - 对话式 AI 助手可将数百万数据点即时转化为可执行洞察
- ⚡ **实时监控** - 从每小时到每天的刷新频率，并提供即时告警（email、Slack、webhook）
- 🌍 **无限扩展** - 任何网站、任何地区、任何刷新频率
- 🔗 **即插即用集成** - AWS、GCP、Databricks、Snowflake 等
- 🛡️ **全托管** - Bright Data 自动处理 schema 变更、网站更新和数据质量问题

**关键使用场景：**
- ✅ **监控 Farfetch 上稀有和限量版商品的价格**
- ✅ **实时追踪库存可用性**，覆盖不同尺码和颜色
- ✅ **验证转售溢价** 与零售价之间的差异
- ✅ 监控 MAP 政策合规性并检测价格违规
- ✅ 追踪竞争对手促销和促销动态
- ✅ 将干净、统一的数据直接输入动态定价算法或 AI 模型

> **起价 $250/月 - [获取定制报价 →](https://bright.cn/products/insights/price-tracker/farfetch)**

---

## 选项 2：构建你自己的 Farfetch Scraper

没有预构建的 Farfetch scraper API？没问题。Bright Data 的 **AI Scraper Builder** 只需几次点击即可生成自定义 Farfetch scraper——无需编写代码。

### 几分钟内构建你的 Farfetch scraper

**[打开 Farfetch AI Scraper Builder →](https://bright.cn/products/web-scraper/farfetch)**

选择域名，描述你的数据需求，让我们的 AI scraper builder 自动创建 API。

1. **用自然语言描述数据需求**
2. **AI 即时生成 scraper API**
3. **运行 API 请求并立即获得结果**
4. **如有需要，可在内置 IDE 中编辑代码**

构建完成后，你的 scraper 会获得一个 **Web Scraper ID** (`gd_xxxxxxxxxxxx`)——复制它，用于下面的 Setup 步骤。

### 前置条件

- Python 3.9 或更高版本
- 一个 [Bright Data account](https://bright.cn)（提供免费试用）
- 一个 Bright Data **API token**（[如何获取](https://docs.bright.cn/general/account/account-settings#api-token)）
- 一个用于 Farfetch 的 **Web Scraper ID**（来自上面的构建步骤）

### Setup

1. **克隆此 repository**

   ```bash
   git clone https://github.com/bright-cn/farfetch-price-tracker.git
   cd farfetch-price-tracker
   ```

2. **安装依赖**

   ```bash
   pip install -r requirements.txt
   ```

3. **配置凭证**

   将 `.env.example` 复制为 `.env` 并填写你的值：

   ```bash
   cp .env.example .env
   ```

   ```env
   BRIGHTDATA_API_TOKEN=your_api_token_here
   BRIGHTDATA_DATASET_ID=your_dataset_id_here
   ```

   > **你的 Web Scraper ID**
   > 将来自你的 [AI Scraper Builder dashboard](https://bright.cn/products/web-scraper/farfetch) 的 Web Scraper ID
   > 粘贴到 `BRIGHTDATA_DATASET_ID` 中（格式：`gd_xxxxxxxxxxxx`）。

---

## 用法

当你的 Farfetch scraper 构建完成，并且你的 Web Scraper ID 已在 `.env` 中配置后，Python 接口的使用方式相同：

### 1. 按 URL 追踪特定商品

传入 Farfetch 商品 URL 列表以获取结构化价格数据：

```python
from price_tracker import track_prices

urls = [
    "https://www.farfetch.com/en/products/sample-product-123456",
    # Add more product URLs here
]

results = track_prices(urls)
for item in results:
    print(f"{item.get('title')} - {item.get('final_price', item.get('price'))} {item.get('currency', '')}")
```

或直接运行：

```bash
python price_tracker.py
```

### 2. 按关键词发现商品

查找与关键词搜索匹配的商品：

```python
from price_tracker import discover_by_keyword

results = discover_by_keyword("laptop", limit=50)
```

### 3. 按分类 URL 浏览商品

从 Farfetch 分类页面采集所有商品：

```python
from price_tracker import discover_by_category

results = discover_by_category(
    "https://farfetch.com/category/example",
    limit=100,
)
```

---

## 输出字段

每条结果记录包含以下字段：

| Field | Description |
|-------|-------------|
| `url` | 商品页面 URL |
| `title` | 商品名称 |
| `brand` | 品牌/时装屋 |
| `price` | 零售价 |
| `currency` | 货币代码 |
| `in_stock` | 库存状态 |
| `color` | 颜色 |
| `size` | 可选尺码 |
| `material` | 使用材质 |
| `sku` | SKU / 参考编号 |
| `images` | 商品图片 |
| `description` | 商品描述 |
| `timestamp` | 采集时间戳 |

### 示例输出

```json
[
  {
    "url": "https://www.farfetch.com/en/products/sample-product-123456",
    "title": "Example Product Name",
    "brand": "Example Brand",
    "initial_price": 59.99,
    "final_price": 44.99,
    "currency": "USD",
    "discount": "25%",
    "in_stock": true,
    "rating": 4.5,
    "reviews_count": 1234,
    "images": ["https://farfetch.com/images/product1.jpg"],
    "description": "Product description text...",
    "timestamp": "2025-01-15T10:30:00Z"
  }
]
```

---

## 高级选项

`trigger_collection()` 函数接受可选参数来控制数据采集：

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | - | 返回记录的最大数量 |
| `include_errors` | boolean | `true` | 在结果中包含错误报告 |
| `notify` | string (URL) | - | 当快照准备就绪时调用的 webhook URL |
| `format` | string | `json` | 输出格式：`json`、`csv` 或 `ndjson` |

选项示例：

```python
from price_tracker import trigger_collection, get_results

inputs = [{"url": "https://www.farfetch.com/en/products/sample-product-123456"}]
snapshot_id = trigger_collection(inputs, limit=200, notify="https://your-webhook.com/hook")
results = get_results(snapshot_id)
```

---

## 资源

- 🌟 [Farfetch Price Tracker - Bright Insights (Managed)](https://bright.cn/products/insights/price-tracker/farfetch)
- 🏗️ [Build a Farfetch Scraper](https://bright.cn/products/web-scraper/farfetch)
- 📖 [Bright Data Web Scraper API Documentation](https://docs.bright.cn/scraping-automation/web-scraper-api/overview)
- 🗄️ [Web Scrapers Control Panel](https://bright.cn/cp/scrapers)
- 🔑 [How to get an API token](https://docs.bright.cn/general/account/account-settings#api-token)
- 🌐 [Bright Data Homepage](https://bright.cn)

---

*使用 [Bright Data](https://bright.cn) 构建——行业领先的 web 数据平台。*