# 📝 更新日誌

**Changelog**

本文件記錄專案的所有重要更新。

---

## 格式說明

本專案遵循 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.0.0/) 格式，
版本號遵循 [語意化版本](https://semver.org/lang/zh-CN/)。

### 版本類型

- `MAJOR` (主版本號)：不相容的 API 變更
- `MINOR` (次版本號)：向後相容的功能新增
- `PATCH` (修訂號)：向後相容的問題修正

### 變更類型

- `Added` - 新增功能
- `Changed` - 變更現有功能
- `Deprecated` - 即將移除的功能
- `Removed` - 已移除的功能
- `Fixed` - 錯誤修正
- `Security` - 安全性修正

---

## [未發布]

### Added
- 初始專案結構
- README.md 專案說明文件
- docs/DESIGN.md 系統設計文件
- docs/API.md 網站結構分析文件
- docs/CHANGELOG.md 更新日誌

### Changed
- 無

### Fixed
- 無

---

## [0.1.0] - 2026-03-13

### Added
- **專案初始化**
  - 建立 GitHub 倉庫
  - 設定專案結構
  - 建立基礎文件

- **文件系統**
  - README.md - 完整的專案說明文件
  - docs/DESIGN.md - 詳細的系統設計文件
  - docs/API.md - 目標網站結構分析
  - docs/CHANGELOG.md - 更新日誌

- **系統設計**
  - 定義系統架構（三層架構）
  - 設計模組劃分（Crawler、Detector、Notifier、Storage）
  - 資料庫設計（SQLite，3 個資料表）
  - 錯誤處理機制
  - 效能優化策略
  - 安全性考量

- **技術選型**
  - Python 3.8+ 作為主要開發語言
  - requests + BeautifulSoup 作為爬蟲工具
  - SQLite 作為資料庫
  - Telegram/Email 作為通知方式
  - Cron/Heartbeat 作為定期執行機制

- **網站分析**
  - 完成目標網站 HTML 結構分析
  - 定義資料欄位提取方法
  - 提供 CSS 選擇器參考
  - 提供多種語言的請求範例
  - 定義分頁處理策略

### Changed
- 無

### Fixed
- 無

### Security
- 定義敏感資料保護方案
- 使用環境變數儲存憑證
- 限制檔案權限

---

## 未來計劃

### [0.2.0] - 預計 2026-03-20
- 實現基礎爬蟲模組
- 實現資料庫模組
- 實現基礎通知功能
- 加入配置管理

### [0.3.0] - 預計 2026-03-27
- 實現新公告檢測邏輯
- 加入定期執行機制
- 加入日誌記錄
- 完善錯誤處理

### [0.4.0] - 預計 2026-04-03
- 加入多類別監控支援
- 加入數據匯出功能（CSV/JSON）
- 加入簡單統計功能
- 優化效能

### [1.0.0] - 預計 2026-04-10
- 完整功能實現
- 完整測試覆蓋
- 完整文件
- 正式發布

---

## 版本歷程

| 版本 | 日期 | 狀態 | 說明 |
|------|------|------|------|
| 0.1.0 | 2026-03-13 | ✅ 已完成 | 專案初始化，文件完成 |
| 0.2.0 | 預計 2026-03-20 | 📋 計劃中 | 基礎功能實現 |
| 0.3.0 | 預計 2026-03-27 | 📋 計劃中 | 完整功能實現 |
| 0.4.0 | 預計 2026-04-03 | 📋 計劃中 | 功能增強 |
| 1.0.0 | 預計 2026-04-10 | 📋 計劃中 | 正式發布 |

---

## 貢獻者

| 貢獻者 | GitHub | 貢獻內容 |
|--------|--------|----------|
| john-fb-agent | @john-fb-agent | 專案發起人、主要開發者 |

---

## 相關連結

- [GitHub 倉庫](https://github.com/john-fb-agent/macao-announcement-monitor)
- [問題追蹤](https://github.com/john-fb-agent/macao-announcement-monitor/issues)
- [專案文件](./README.md)
- [系統設計](./DESIGN.md)
- [網站分析](./API.md)

---

<div align="center">

**最後更新**：2026-03-13

**保持更新，追蹤最新變更！**

</div>
