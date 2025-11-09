# ClickHouse Docker 部署

本專案提供 ClickHouse 的 Docker Compose 部署配置。

## 📊 ClickHouse 簡介

ClickHouse 是一個開源的列式資料庫管理系統（DBMS），專為線上分析處理（OLAP）而設計。

### 主要特性
- 🚀 極快的查詢速度
- 📈 優秀的壓縮率
- 🔄 支援實時數據插入
- 📊 適合大數據分析和日誌分析

## 🚀 快速開始

### 前置需求
- Docker
- Docker Compose

### 啟動服務

```bash
# 啟動 ClickHouse
docker-compose up -d

# 查看日誌
docker-compose logs -f

# 停止服務
docker-compose down
```

## 🔌 連接資訊

### 端口說明

| 用途 | 協定 | 端口 | 說明 |
|------|------|------|------|
| **Native Client** | TCP | **9000** | 給 clickhouse-client 或應用程式使用（最常見，效能最佳）|
| HTTP API | HTTP | 8123 | 給 REST API 或瀏覽器使用 |
| Interserver | TCP | 9009 | 節點間同步資料（複寫用）|
| Prometheus Metrics | HTTP | 9363 | 監控系統統計資訊 |

### 預設認證

- **使用者**: `default`
- **密碼**: `clickhouse`
- **資料庫**: `default`

## 💻 使用範例

### 使用 clickhouse-client

```bash
# 進入容器
docker exec -it clickhouse-server clickhouse-client

# 或直接執行查詢
docker exec -it clickhouse-server clickhouse-client --query "SELECT version()"
```

### 使用 HTTP API

```bash
# 查詢版本
curl 'http://localhost:8123/?query=SELECT%20version()'

# 執行查詢
curl 'http://localhost:8123/' --data-binary "SELECT 1"

# 查看資料庫列表
curl 'http://localhost:8123/?query=SHOW%20DATABASES'
```

### Python 範例

```python
from clickhouse_driver import Client

client = Client(
    host='localhost',
    port=9000,
    user='default',
    password='clickhouse'
)

# 查詢
result = client.execute('SELECT version()')
print(result)
```

## 📁 目錄結構

```
Clickhouse/
├── docker-compose.yml    # Docker Compose 配置
├── README.md            # 本文件
└── .gitignore          # Git 忽略規則
```

## 🔧 配置說明

### 環境變數

在 `docker-compose.yml` 中配置：

- `CLICKHOUSE_DB`: 預設資料庫名稱
- `CLICKHOUSE_USER`: 預設使用者名稱
- `CLICKHOUSE_PASSWORD`: 預設密碼
- `CLICKHOUSE_DEFAULT_ACCESS_MANAGEMENT`: 啟用存取管理

### 資源限制

- `nofile`: 檔案描述符限制設為 262144

## 📊 資料持久化

資料儲存在 Docker volumes：
- `clickhouse_data`: ClickHouse 資料檔案
- `clickhouse_logs`: ClickHouse 日誌檔案

## 🛠️ 常用指令

```bash
# 查看容器狀態
docker-compose ps

# 查看即時日誌
docker-compose logs -f clickhouse

# 重啟服務
docker-compose restart

# 進入容器 shell
docker exec -it clickhouse-server bash

# 備份資料
docker-compose exec clickhouse clickhouse-client --query "BACKUP DATABASE default TO Disk('backups', 'backup.zip')"
```

## 📝 注意事項

1. **生產環境**: 請修改預設密碼
2. **效能調優**: 根據硬體資源調整配置
3. **安全性**: 建議設定防火牆規則限制訪問
4. **備份**: 定期備份重要資料

## 🔗 相關連結

- [ClickHouse 官方文檔](https://clickhouse.com/docs)
- [ClickHouse GitHub](https://github.com/ClickHouse/ClickHouse)
- [Docker Hub - ClickHouse](https://hub.docker.com/r/clickhouse/clickhouse-server)

## 📄 授權

本配置檔案可自由使用和修改。

---

*最後更新: 2025-11-09*
