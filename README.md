# 🇲🇴 澳門公開競投公告監控系統

**Macao Tender Announce**

自動監控澳門特別行政區公報 - 公開競投公告，即時通知新公告發布。

---

## 📋 專案說明

本系統定期抓取 [澳門特別行政區公報](https://www.bo.dsaj.gov.mo) 的公開競投公告，記錄並追蹤新公告，當發現新公告時自動通知用戶。

### 監控目標

- **網站**：澳門特別行政區政府法務局 - 公報
- **網址**：https://www.bo.dsaj.gov.mo/cn/news/list/b/?d=13
- **類別**：公開競投 (服務提供/物品供應)
- **更新頻率**：不定期（政府部門自行發布）

---

## ✨ 功能特點

| 功能 | 說明 |
|------|------|
| 🔍 **自動抓取** | 定期抓取公報網站的最新公告 |
| 📊 **新公告檢測** | 自動比對並識別新公告 |
| 📢 **即時通知** | 發現新公告時立即通知用戶 |
| 💾 **歷史記錄** | 儲存所有公告記錄，支援查詢 |
| 📈 **數據分析** | 統計各部門公告數量、趨勢分析 |

---

## 🎯 適用場景

- ✅ 政府採購供應商 - 即時獲取招標資訊
- ✅ 承辦商 - 不錯過任何競投機會
- ✅ 研究者 - 分析政府採購趨勢
- ✅ 一般市民 - 了解政府資源分配

---

## 🏗️ 系統架構

```
┌─────────────────────────────────────────────────────┐
│                   用戶通知層                          │
│              (Telegram / Email / Log)               │
└─────────────────────────────────────────────────────┘
                          ▲
                          │
┌─────────────────────────────────────────────────────┐
│                   應用邏輯層                          │
│   ┌──────────┐  ┌──────────┐  ┌──────────┐         │
│   │ 抓取模組  │  │ 檢測模組  │  │ 通知模組  │         │
│   └──────────┘  └──────────┘  └──────────┘         │
└─────────────────────────────────────────────────────┘
                          ▲
                          │
┌─────────────────────────────────────────────────────┐
│                   資料儲存層                          │
│              (SQLite / JSON / CSV)                  │
└─────────────────────────────────────────────────────┘
                          ▲
                          │
┌─────────────────────────────────────────────────────┐
│                   數據來源層                          │
│        https://www.bo.dsaj.gov.mo                   │
└─────────────────────────────────────────────────────┘
```

---

## 📁 專案結構

```
macao-announcement-monitor/
├── README.md              # 本文件
├── docs/
│   ├── DESIGN.md          # 系統設計文件
│   ├── API.md             # 網站結構分析
│   └── CHANGELOG.md       # 更新日誌
├── src/                   # 程式碼
│   ├── main.py           # 主程式
│   ├── crawler.py        # 爬蟲模組
│   ├── detector.py       # 檢測模組
│   ├── notifier.py       # 通知模組
│   └── database.py       # 資料庫模組
├── config/
│   └── config.json       # 配置文件
├── logs/                  # 日誌檔案
├── data/                  # 資料儲存
├── requirements.txt       # Python 依賴
└── .gitignore            # Git 忽略檔案
```

---

## 🚀 快速開始

### 環境需求

- Python 3.8+
- pip
- Git

### 安裝步驟

```bash
# 1. 克隆倉庫
git clone https://github.com/john-fb-agent/macao-announcement-monitor.git
cd macao-announcement-monitor

# 2. 安裝依賴
pip install -r requirements.txt

# 3. 配置設定
cp config/config.example.json config/config.json
# 編輯 config/config.json 填入你的設定

# 4. 執行程式
python src/main.py
```

### 定期執行

#### 使用 Cron（Linux/Mac）

```bash
# 編輯 crontab
crontab -e

# 添加以下行（每天早上 9 點執行）
0 9 * * * /usr/bin/python3 /path/to/macao-announcement-monitor/src/main.py
```

#### 使用 Heartbeat（OpenClaw）

在 `HEARTBEAT.md` 中添加：

```bash
python3 /root/.openclaw/workspace/macao-announcement-monitor/src/main.py
```

---

## 📊 數據欄位

每個公告包含以下資訊：

| 欄位 | 說明 | 範例 |
|------|------|------|
| `link_id` | 公告唯一 ID | `98152` |
| `department` | 發佈部門 | `交通事務局` |
| `description` | 公告內容摘要 | `公共停車場泊車服務經營批給` |
| `gazette_no` | 公報期數 | `第 10 期` |
| `gazette_group` | 公報組別 | `第二組` |
| `publish_date` | 發佈日期 | `2026/03/11` |
| `url` | 詳細內容連結 | `/cn/bo/b/link/98152` |

---

## 🔧 配置選項

| 選項 | 說明 | 預設值 |
|------|------|--------|
| `check_interval` | 檢查間隔（分鐘） | `30` |
| `notify_method` | 通知方式 | `telegram` |
| `storage_type` | 儲存類型 | `sqlite` |
| `max_pages` | 抓取頁數上限 | `1` |
| `log_level` | 日誌級別 | `INFO` |

---

## 📝 使用範例

### 查詢最新公告

```python
from src.database import Database

db = Database('data/announcements.db')
latest = db.get_latest(10)  # 獲取最新 10 筆

for item in latest:
    print(f"{item['date']} - {item['department']}: {item['description']}")
```

### 匯出為 CSV

```python
from src.database import Database
import csv

db = Database('data/announcements.db')
all_items = db.get_all()

with open('export.csv', 'w', newline='', encoding='utf-8') as f:
    writer = csv.DictWriter(f, fieldnames=all_items[0].keys())
    writer.writeheader()
    writer.writerows(all_items)
```

---

## 🧪 測試

```bash
# 執行測試
pytest tests/

# 測試爬蟲模組
python tests/test_crawler.py

# 測試資料庫模組
python tests/test_database.py
```

---

## 📚 文件

| 文件 | 說明 |
|------|------|
| [README.md](README.md) | 專案說明（本文件） |
| [docs/DESIGN.md](docs/DESIGN.md) | 系統設計與架構 |
| [docs/API.md](docs/API.md) | 目標網站結構分析 |
| [docs/CHANGELOG.md](docs/CHANGELOG.md) | 版本更新日誌 |

---

## ⚠️ 注意事項

1. **使用條款**：請遵守目標網站的使用條款，避免過度頻繁請求
2. **資料準確性**：本系統僅供參考，請以官方公報為準
3. **隱私保護**：請勿濫用獲取的資訊
4. **錯誤處理**：系統可能因網站結構改變而失效，請定期檢查

---

## 🤝 貢獻

歡迎提交 Issue 和 Pull Request！

1. Fork 本專案
2. 建立功能分支 (`git checkout -b feature/AmazingFeature`)
3. 提交變更 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 開啟 Pull Request

---

## 📄 授權

本專案採用 [MIT License](LICENSE) 授權。

---

## 📞 聯絡

- **作者**：john-fb-agent
- **專案連結**：https://github.com/john-fb-agent/macao-tender-announce
- **問題回報**：https://github.com/john-fb-agent/macao-tender-announce/issues

---

## 🙏 致謝

- 資料來源：[澳門特別行政區政府法務局](https://www.bo.dsaj.gov.mo)
- 靈感來源：政府採購透明化倡議

---

<div align="center">

**⭐ 如果這個專案對你有幫助，請給個星星支持一下！**

Made with ❤️ for Macao

</div>
