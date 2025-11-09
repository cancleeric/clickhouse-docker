# Changelog

本文件記錄 ClickHouse Docker 配置的所有重要變更。

格式基於 [Keep a Changelog](https://keepachangelog.com/zh-TW/1.0.0/)。

---

## [1.1.0] - 2025-11-09

### 🔧 變更 (Changed)

#### **從 Docker Named Volumes 改為主機目錄映射**

**原因**: 為了更方便地訪問和管理 ClickHouse 數據，提供更好的開發體驗。

**變更內容**:

1. **docker-compose.yml**
   - 移除 `volumes:` 區段中的 named volumes 定義
   - 將 volume 映射從 named volumes 改為本地目錄：
     ```yaml
     # 之前
     volumes:
       - clickhouse_data:/var/lib/clickhouse
       - clickhouse_logs:/var/log/clickhouse-server

     # 之後
     volumes:
       - ./data:/var/lib/clickhouse
       - ./logs:/var/log/clickhouse-server
     ```

2. **.gitignore**
   - 新增 `backup/` 到忽略清單
   - 確保 `data/` 和 `logs/` 已在忽略清單中

3. **README.md**
   - 更新目錄結構說明
   - 更新資料持久化章節
   - 新增備份建議

**遷移步驟**:
1. ✅ 備份現有數據 (121MB)
2. ✅ 停止 ClickHouse 容器
3. ✅ 更新 docker-compose.yml
4. ✅ 創建本地目錄 `data/` 和 `logs/`
5. ✅ 恢復數據到本地目錄
6. ✅ 重新啟動容器
7. ✅ 驗證服務正常運行

**驗證結果**:
- 容器狀態: healthy ✅
- 數據完整性: 413MB 數據成功遷移 ✅
- 服務連接: ClickHouse 25.10.1.3832 正常運行 ✅
- Volume 類型: bind (主機目錄映射) ✅

**優勢**:
- ✅ 直接從本機文件系統訪問資料庫文件
- ✅ 使用標準文件系統工具即可備份
- ✅ 可在不同開發環境間共享資料
- ✅ 刪除容器不會丟失資料

**注意事項**:
- 舊的 Docker named volumes (`clickhouse_clickhouse_data`, `clickhouse_clickhouse_logs`) 已不再使用
- 如需清理舊 volumes: `docker volume rm clickhouse_clickhouse_data clickhouse_clickhouse_logs`

---

## [1.0.0] - 2025-11-08

### 🎉 初始版本

- ClickHouse Docker Compose 配置
- 使用 Docker named volumes 儲存數據
- 預設認證配置
- 基礎 README 文檔
