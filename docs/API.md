# 🌐 目標網站結構分析

**Target Website API Documentation**

本文檔詳細分析澳門特別行政區公報網站的結構、HTML 元素和資料提取方法。

---

## 📋 目錄

1. [網站概述](#1-網站概述)
2. [目標頁面](#2-目標頁面)
3. [HTML 結構分析](#3-html 結構分析)
4. [資料欄位提取](#4-資料欄位提取)
5. [CSS 選擇器參考](#5-css 選擇器參考)
6. [請求範例](#6-請求範例)
7. [資料格式](#7-資料格式)
8. [分页處理](#8-分頁處理)
9. [限制與注意事項](#9-限制與注意事項)
10. [常見問題](#10-常見問題)

---

## 1. 網站概述

### 1.1 基本資訊

| 項目 | 說明 |
|------|------|
| **網站名稱** | 澳門特別行政區政府法務局 - 公報 |
| **主辦單位** | 澳門特別行政區政府法務局 (DSAJ) |
| **網址** | https://www.bo.dsaj.gov.mo |
| **語言** | 繁體中文、葡萄牙文 |
| **技術棧** | ASP.NET, JavaScript, CSS |

### 1.2 網站特點

| 特點 | 說明 |
|------|------|
| **靜態 HTML** | 主要內容為伺服器端渲染，無需 JavaScript |
| **傳統表單** | 使用傳統 HTML 表單提交 |
| **無 API** | 無公開 REST API，需解析 HTML |
| **無認證** | 公開資訊，無需登入即可訪問 |

---

## 2. 目標頁面

### 2.1 主要監控頁面

**公開競投 (服務提供/物品供應)**

| 項目 | 說明 |
|------|------|
| **URL** | https://www.bo.dsaj.gov.mo/cn/news/list/b/?d=13 |
| **參數** | `d=13` (類別 ID：公開競投 - 服務/物品) |
| **更新頻率** | 不定期（政府部門自行發布） |
| **內容** | 政府部門發布的公開競投公告 |

### 2.2 其他可監控類別

| 類別 ID | 類別名稱 | URL |
|--------|----------|-----|
| 11 | 入職開考 | `/cn/news/list/b/?d=11` |
| 10 | 晉升開考 (有限制方式) | `/cn/news/list/b/?d=10` |
| 12 | 公開競投 (工程承包) | `/cn/news/list/b/?d=12` |
| **13** | **公開競投 (服務提供/物品供應)** | `/cn/news/list/b/?d=13` |
| 23 | 拍賣 | `/cn/news/list/b/?d=23` |
| 31 | 商業登記 | `/cn/news/list/b/?d=31` |
| 29 | 工業產權的保護 | `/cn/news/list/b/?d=29` |
| 15 | 稅務 | `/cn/news/list/b/?d=15` |

---

## 3. HTML 結構分析

### 3.1 整體結構

```html
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <!-- 元數據、CSS、JS -->
</head>
<body>
    <div class="container">
        <!-- 頁頭 -->
        <div class="row headerback">...</div>
        
        <!-- 導航欄 -->
        <div class="row" style="background-color:white">
            <nav class="navbar navbar-default">...</nav>
        </div>
        
        <!-- 主要內容 -->
        <div class="row" style="background-color:white">
            <div id="content" class="col-md-12 col-lg-12">
                <div class="">
                    <!-- 麵包屑導航 -->
                    <ol class="breadcrumb">...</ol>
                    
                    <!-- 篩選表單 -->
                    <form action="/cn/news/list/b/" role="search">...</form>
                    
                    <!-- 分頁 -->
                    <div class="text-center">
                        <ul class="pagination">...</ul>
                    </div>
                    
                    <!-- 公告列表表格 -->
                    <Table Class="table table-striped">
                        <thead>
                            <tr>
                                <th>摘要</th>
                                <th class="text-right">《公報》編號：</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td class="col-md-9">...</td>
                                <td class="col-md-3 text-right text-nowrap">...</td>
                            </tr>
                            <!-- 更多公告... -->
                        </tbody>
                    </Table>
                    
                    <!-- 分頁（底部） -->
                    <div class="text-center">
                        <ul class="pagination">...</ul>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- 頁腳 -->
        <div class="footer">...</div>
    </div>
</body>
</html>
```

### 3.2 公告列表表格結構

```html
<Table Class="table table-striped">
    <thead>
        <tr>
            <th>摘要</th>
            <th Class="text-right">《公報》編號：</th>
        </tr>
    </thead>
    <tbody>
        <!-- 單條公告 -->
        <tr>
            <!-- 左側：部門和內容 -->
            <td class="col-md-9">
                <a href="/cn/bo/b/link/98152">交通事務局</a>
                , 公告一則，關於祐漢公園、海灣南街、澳門文化中心、
                聯生圓形地、澳門協和醫院及望信樓的公共停車場
                公共泊車服務經營批給的公開競投。
            </td>
            
            <!-- 右側：公報編號和日期 -->
            <td Class="col-md-3 text-right text-nowrap">
                <a class='' href='https://bo.dsaj.gov.mo/bo/ii/2026/10/bo10_cn.asp'>
                    《公報》第 10 期，<br>第二組，<br>2026/03/11
                </a>
            </td>
        </tr>
        <!-- 更多公告... -->
    </tbody>
</Table>
```

---

## 4. 資料欄位提取

### 4.1 欄位映射

| 欄位 | HTML 來源 | 提取方法 | 範例 |
|------|----------|----------|------|
| `link_id` | `<a href="/cn/bo/b/link/98152">` | 從 URL 提取數字 | `98152` |
| `department` | `<a>交通事務局</a>` | 第一個 `<a>` 標籤文字 | `交通事務局` |
| `description` | `<td>` 剩餘文字 | 移除部門後的文字 | `公告一則，關於...` |
| `gazette_no` | `《公報》第 10 期` | 從右側单元格提取 | `第 10 期` |
| `gazette_group` | `第二組` | 從右側单元格提取 | `第二組` |
| `publish_date` | `2026/03/11` | 從右側单元格提取 | `2026/03/11` |
| `url` | `/cn/bo/b/link/98152` | 完整 URL | `/cn/bo/b/link/98152` |

### 4.2 提取範例（Python）

```python
from bs4 import BeautifulSoup

def parse_announcement_row(row):
    """解析單條公告行"""
    cells = row.find_all('td')
    if len(cells) != 2:
        return None
    
    left_cell = cells[0]
    right_cell = cells[1]
    
    # 提取部門（第一個 <a> 標籤）
    department_link = left_cell.find('a')
    if not department_link:
        return None
    
    department = department_link.get_text(strip=True)
    
    # 提取 link_id
    link_href = department_link.get('href', '')
    link_id = link_href.split('/link/')[-1] if '/link/' in link_href else None
    
    # 提取內容描述（移除部門後的文字）
    description = left_cell.get_text(strip=True)
    description = description.replace(department, '', 1).strip(' ,.，。')
    
    # 提取公報資訊
    right_text = right_cell.get_text(strip=True)
    # 範例：《公報》第 10 期，\n第二組，\n2026/03/11
    right_parts = [p.strip() for p in right_text.split(',') if p.strip()]
    
    gazette_no = ''
    gazette_group = ''
    publish_date = ''
    
    for part in right_parts:
        if '第' in part and '期' in part:
            gazette_no = part.replace('《公報》', '').strip()
        elif '組' in part:
            gazette_group = part
        elif '/' in part and len(part) == 10:  # 2026/03/11
            publish_date = part
    
    return {
        'link_id': link_id,
        'department': department,
        'description': description,
        'gazette_no': gazette_no,
        'gazette_group': gazette_group,
        'publish_date': publish_date,
        'url': link_href
    }

# 使用範例
soup = BeautifulSoup(html_content, 'lxml')
table = soup.find('table', class_='table-striped')
rows = table.find_all('tr')[1:]  # 跳過表頭

announcements = []
for row in rows:
    data = parse_announcement_row(row)
    if data:
        announcements.append(data)
```

---

## 5. CSS 選擇器參考

### 5.1 主要選擇器

| 元素 | CSS 選擇器 | 說明 |
|------|-----------|------|
| 公告表格 | `table.table-striped` | 公告列表表格 |
| 公告行 | `table.table-striped tr` | 每一行公告 |
| 表頭行 | `table.table-striped thead tr` | 表頭 |
| 數據行 | `table.table-striped tbody tr` | 數據行 |
| 左側单元格 | `td.col-md-9` | 部門和內容 |
| 右側单元格 | `td.col-md-3` | 公報資訊 |
| 部門連結 | `td.col-md-9 a` | 部門名稱連結 |
| 分頁連結 | `ul.pagination li a` | 分頁連結 |

### 5.2 BeautifulSoup 選擇器

```python
# 獲取所有公告行
rows = soup.select('table.table-striped tbody tr')

# 獲取第一條公告
first_row = soup.select_one('table.table-striped tbody tr')

# 獲取部門名稱
department = soup.select_one('table.table-striped tbody tr td a').text

# 獲取所有分頁連結
page_links = soup.select('ul.pagination li a')
```

### 5.3 lxml XPath

```python
from lxml import html

tree = html.fromstring(html_content)

# 獲取所有公告行
rows = tree.xpath('//table[@class="table-striped"]//tr')

# 獲取第一條公告的部門
department = tree.xpath('//table[@class="table-striped"]//tr[2]/td[1]/a/text()')[0]

# 獲取公告數量
count = len(tree.xpath('//table[@class="table-striped"]//tbody/tr'))
```

---

## 6. 請求範例

### 6.1 Python requests

```python
import requests

def fetch_announcements(page=1, category_id=13):
    """抓取公告列表"""
    url = "https://www.bo.dsaj.gov.mo/cn/news/list/b/"
    params = {
        'd': category_id,  # 類別 ID
        'p': page          # 頁碼
    }
    
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36',
        'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8',
        'Accept-Language': 'zh-TW,zh;q=0.9,en-US;q=0.8,en;q=0.7',
    }
    
    response = requests.get(url, params=params, headers=headers, timeout=30)
    response.raise_for_status()
    
    return response.text
```

### 6.2 cURL

```bash
# 抓取第 1 頁
curl -L "https://www.bo.dsaj.gov.mo/cn/news/list/b/?d=13&p=1" \
  -H "User-Agent: Mozilla/5.0" \
  -H "Accept: text/html" \
  -o announcements_page1.html

# 抓取第 2 頁
curl -L "https://www.bo.dsaj.gov.mo/cn/news/list/b/?d=13&p=2" \
  -H "User-Agent: Mozilla/5.0" \
  -H "Accept: text/html" \
  -o announcements_page2.html
```

### 6.3 Node.js (axios)

```javascript
const axios = require('axios');

async function fetchAnnouncements(page = 1, categoryId = 13) {
  const url = 'https://www.bo.dsaj.gov.mo/cn/news/list/b/';
  const params = { d: categoryId, p: page };
  
  const headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36',
    'Accept': 'text/html,application/xhtml+xml',
  };
  
  const response = await axios.get(url, { params, headers });
  return response.data;
}
```

---

## 7. 資料格式

### 7.1 原始 HTML 片段

```html
<tr>
    <td class="col-md-9">
        <a href="/cn/bo/b/link/98152">交通事務局</a>, 公告一則，關於祐漢公園、海灣南街、澳門文化中心、聯生圓形地、澳門協和醫院及望信樓的公共停車場公共泊車服務經營批給的公開競投。
    </td>
    <td Class="col-md-3 text-right text-nowrap">
        <a class='' href='https://bo.dsaj.gov.mo/bo/ii/2026/10/bo10_cn.asp'>
            《公報》第 10 期，<br>第二組，<br>2026/03/11
        </a>
    </td>
</tr>
```

### 7.2 提取後的 JSON 格式

```json
{
  "link_id": "98152",
  "department": "交通事務局",
  "description": "公告一則，關於祐漢公園、海灣南街、澳門文化中心、聯生圓形地、澳門協和醫院及望信樓的公共停車場公共泊車服務經營批給的公開競投。",
  "gazette_no": "第 10 期",
  "gazette_group": "第二組",
  "publish_date": "2026/03/11",
  "url": "/cn/bo/b/link/98152",
  "full_url": "https://www.bo.dsaj.gov.mo/cn/bo/b/link/98152",
  "gazette_url": "https://bo.dsaj.gov.mo/bo/ii/2026/10/bo10_cn.asp"
}
```

### 7.3 完整公告列表

```json
{
  "page": 1,
  "total_pages": 42,
  "total_count": 4140,
  "category_id": 13,
  "category_name": "公開競投 (服務提供/物品供應)",
  "fetched_at": "2026-03-13T10:04:00+08:00",
  "announcements": [
    {
      "link_id": "98152",
      "department": "交通事務局",
      "description": "...",
      "gazette_no": "第 10 期",
      "gazette_group": "第二組",
      "publish_date": "2026/03/11",
      "url": "/cn/bo/b/link/98152"
    },
    {
      "link_id": "98134",
      "department": "衛生局",
      "description": "...",
      "gazette_no": "第 10 期",
      "gazette_group": "第二組",
      "publish_date": "2026/03/11",
      "url": "/cn/bo/b/link/98134"
    }
    // 更多公告...
  ]
}
```

---

## 8. 分頁處理

### 8.1 分頁結構

```html
<div class="text-center">
    <ul class="pagination">
        <li class="active"><a href="/cn/news/list/b/?p=1&d=13">1</a></li>
        <li><a href="/cn/news/list/b/?p=2&d=13">2</a></li>
        <li><a href="/cn/news/list/b/?p=3&d=13">3</a></li>
        <li><a href="/cn/news/list/b/?p=4&d=13">4</a></li>
        <li><a href="/cn/news/list/b/?p=5&d=13">5</a></li>
        <li><a href="/cn/news/list/b/?p=6&d=13">»</a></li>
        <li><a href="/cn/news/list/b/?p=42&d=13">→</a></li>
    </ul>
</div>
```

### 8.2 分頁資訊提取

```python
def parse_pagination(soup):
    """解析分頁資訊"""
    pagination = soup.select_one('ul.pagination')
    if not pagination:
        return None
    
    # 獲取所有頁碼連結
    page_links = pagination.select('li a')
    
    pages = []
    for link in page_links:
        href = link.get('href', '')
        text = link.get_text(strip=True)
        
        # 提取頁碼
        if 'p=' in href:
            page_num = href.split('p=')[1].split('&')[0]
            pages.append({
                'page': int(page_num),
                'text': text,
                'href': href,
                'is_current': 'active' in link.parent.get('class', [])
            })
    
    # 獲取總頁數（最後一頁）
    last_page = pages[-1]['page'] if pages else 1
    
    return {
        'current_page': next((p['page'] for p in pages if p['is_current']), 1),
        'total_pages': last_page,
        'pages': pages
    }
```

### 8.3 多頁抓取策略

```python
def fetch_all_pages(base_url, max_pages=5):
    """抓取多頁公告"""
    all_announcements = []
    
    for page in range(1, max_pages + 1):
        print(f"抓取第 {page} 頁...")
        
        html = fetch_announcements(page=page)
        soup = BeautifulSoup(html, 'lxml')
        
        announcements = parse_announcements(soup)
        all_announcements.extend(announcements)
        
        # 禮貌性延遲
        time.sleep(1)
    
    return all_announcements
```

---

## 9. 限制與注意事項

### 9.1 技術限制

| 限制 | 說明 | 應對方案 |
|------|------|----------|
| **無 API** | 無官方 API，需解析 HTML | 使用 BeautifulSoup 解析 |
| **網站結構改變** | HTML 結構可能改變 | 加入異常處理和監控 |
| **反爬蟲機制** | 可能有 rate limit | 加入請求延遲 |
| **JavaScript** | 部分功能需 JS | 目前主要內容為靜態 HTML |

### 9.2 使用規範

| 規範 | 說明 |
|------|------|
| **請求頻率** | 建議間隔 >= 1 秒 |
| **User-Agent** | 使用真實瀏覽器 User-Agent |
| **並發限制** | 避免多線程併發請求 |
| **資料使用** | 僅供個人使用，勿商業用途 |

### 9.3 錯誤處理

```python
from requests.exceptions import RequestException, Timeout, HTTPError

def safe_fetch(url, max_retries=3):
    """安全抓取，包含重試機制"""
    for attempt in range(max_retries):
        try:
            response = requests.get(url, timeout=30)
            response.raise_for_status()
            return response.text
        except Timeout:
            print(f"請求超時，重試 {attempt + 1}/{max_retries}")
            time.sleep(2 ** attempt)  # 指數退避
        except HTTPError as e:
            if e.response.status_code == 429:  # Rate limited
                print("觸發頻率限制，等待 60 秒...")
                time.sleep(60)
            else:
                raise
        except RequestException as e:
            print(f"請求失敗：{e}")
            if attempt == max_retries - 1:
                raise
            time.sleep(2 ** attempt)
    
    return None
```

---

## 10. 常見問題

### Q1: 為什麼抓取不到資料？

**可能原因**：
1. 網站結構改變
2. 網絡連接問題
3. 被網站封鎖

**解決方案**：
```python
# 1. 檢查 HTML 結構是否改變
print(soup.prettify()[:1000])

# 2. 檢查網絡連接
response = requests.get(url, timeout=10)
print(response.status_code)

# 3. 更換 User-Agent
headers = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36'
}
```

---

### Q2: 如何處理分頁？

**答案**：
```python
# 方法 1：循環抓取
for page in range(1, total_pages + 1):
    fetch_announcements(page=page)

# 方法 2：遞迴抓取
def fetch_recursive(page=1):
    announcements = fetch_announcements(page=page)
    if has_next_page(announcements):
        fetch_recursive(page + 1)
```

---

### Q3: 如何檢測網站結構改變？

**答案**：
```python
def validate_structure(soup):
    """驗證 HTML 結構"""
    table = soup.select_one('table.table-striped')
    if not table:
        raise ValueError("網站結構可能已改變：找不到公告表格")
    
    rows = table.select('tbody tr')
    if not rows:
        raise ValueError("網站結構可能已改變：找不到公告行")
    
    return True
```

---

### Q4: 抓取頻率應該是多少？

**建議**：
- **監控模式**：每 30 分鐘 - 每小時 1 次
- **歷史抓取**：每秒 1 次，加入延遲
- **避免**：高頻並發請求

---

## 📝 修訂記錄

| 版本 | 日期 | 作者 | 說明 |
|------|------|------|------|
| 1.0.0 | 2026-03-13 | john-fb-agent | 初始版本 |

---

<div align="center">

**本文檔基於 2026-03-13 的網站結構分析**

**網站結構可能改變，請定期更新本文檔**

</div>
