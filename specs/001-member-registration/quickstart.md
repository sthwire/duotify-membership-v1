# 快速開始指南

**功能**: 會員註冊流程 | **階段**: 1 (設計) | **日期**: 2025-10-18

## 開發環境需求

### 系統需求
- **Node.js**: 18.0.0 或更新版本
- **npm**: 8.0.0 或更新版本
- **SQL Server**: 2019 或更新版本
- **作業系統**: macOS 12+ / Windows 10+ / Ubuntu 20.04+

### 必要工具
- **Git**: 2.30.0+
- **VS Code**: 推薦安裝 ESLint、Prettier 擴充套件
- **SQL Server Management Studio** (Windows) 或 **Azure Data Studio** (跨平台)

---

## 專案設定

### 1. 複製專案
```bash
git clone <repository-url>
cd duotify-membership-v1
git checkout 001-member-registration
```

### 2. 安裝依賴
```bash
npm install
```

### 3. 環境設定
複製環境設定範本：
```bash
cp .env.example .env
```

編輯 `.env` 檔案：
```env
# 資料庫設定
DB_HOST=localhost
DB_PORT=1433
DB_NAME=duotify_membership
DB_USER=sa
DB_PASSWORD=YourPassword123!

# 郵件服務設定 (使用 SendGrid)
SENDGRID_API_KEY=your-sendgrid-api-key
EMAIL_FROM=noreply@duotify.com
EMAIL_FROM_NAME=Duotify 會員系統

# 應用程式設定
NODE_ENV=development
PORT=3000
SESSION_SECRET=your-super-secret-session-key-here
JWT_SECRET=your-jwt-secret-key-here

# 速率限制設定
RATE_LIMIT_WINDOW_MS=3600000
RATE_LIMIT_MAX_REQUESTS=5
```

### 4. 資料庫設定

#### SQL Server 設定 (macOS 使用 Docker)
```bash
# 使用 Docker 啟動 SQL Server
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=YourPassword123!" \
   -p 1433:1433 --name sqlserver \
   -d mcr.microsoft.com/mssql/server:2019-latest

# 等待 SQL Server 啟動
sleep 30

# 建立資料庫
docker exec -it sqlserver /opt/mssql-tools/bin/sqlcmd \
   -S localhost -U sa -P "YourPassword123!" \
   -Q "CREATE DATABASE duotify_membership;"
```

#### 資料庫遷移
```bash
# 執行資料庫遷移
npm run migrate

# (可選) 載入測試資料
npm run seed
```

---

## 啟動應用程式

### 開發模式
```bash
npm run dev
```

應用程式將在 `http://localhost:3000` 啟動。

### 生產模式
```bash
npm run build
npm start
```

---

## API 測試

### 使用 cURL 測試註冊
```bash
curl -X POST http://localhost:3000/api/v1/registration \
  -H "Content-Type: application/json" \
  -d '{
    "nationalId": "A123456789",
    "name": "測試用戶",
    "email": "test@example.com",
    "password": "Test123456"
  }'
```

### 使用 cURL 測試登入
```bash
curl -X POST http://localhost:3000/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "Test123456"
  }' \
  -c cookies.txt
```

### 使用 Postman
1. 匯入 `specs/001-member-registration/contracts/registration-api.yaml`
2. 設定環境變數：`baseUrl = http://localhost:3000/api/v1`
3. 執行測試集合

---

## 開發工作流程

### 程式碼品質檢查
```bash
# 執行 ESLint
npm run lint

# 自動修復可修復的問題
npm run lint:fix

# 執行測試
npm test

# 測試覆蓋率
npm run test:coverage
```

### 資料庫操作
```bash
# 建立新遷移
npx sequelize-cli migration:generate --name create-new-table

# 執行遷移
npm run migrate

# 復原最後一個遷移
npm run migrate:undo

# 建立 Seed
npx sequelize-cli seed:generate --name demo-users
```

---

## 專案結構說明

```
src/
├── config/          # 設定檔案
├── controllers/     # API 控制器
├── middleware/      # Express 中間件
├── models/          # Sequelize 模型
├── routes/          # API 路由
├── services/        # 業務邏輯
├── utils/           # 工具函數
├── app.js           # Express 應用設定
└── server.js        # 伺服器啟動

tests/               # 測試檔案
├── unit/           # 單元測試
├── integration/    # 整合測試
└── contract/       # 合約測試
```

---

## 常見問題

### 資料庫連線問題
- 確認 SQL Server 正在執行
- 檢查連線字串和認證資訊
- 確認防火牆設定允許 1433 端口

### 郵件發送問題
- 確認 SendGrid API 金鑰正確
- 檢查 API 金鑰權限包含郵件發送
- 確認寄件者郵件地址已驗證

### Session 問題
- 確認 SESSION_SECRET 設定
- 檢查 cookie 設定 (httpOnly, secure)
- 確認資料庫中 Sessions 表存在

---

## 部署指南

### Docker 部署
```bash
# 建置映像
docker build -t duotify-membership .

# 執行容器
docker run -p 3000:3000 \
  -e DB_HOST=host.docker.internal \
  -e DB_PASSWORD=YourPassword123! \
  duotify-membership
```

### 雲端部署
1. 設定生產環境變數
2. 執行資料庫遷移
3. 啟動應用程式
4. 設定反向代理 (Nginx)
5. 設定 SSL 憑證

---

## 相關文件

- [功能規範](spec.md) - 詳細需求說明
- [資料模型](data-model.md) - 資料庫設計
- [API 合約](contracts/) - 完整的 API 文件
- [研究決策](research.md) - 技術決策說明