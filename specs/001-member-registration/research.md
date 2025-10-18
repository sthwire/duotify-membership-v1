# 研究與技術決策

**功能**: 會員註冊流程 | **階段**: 0 (研究) | **日期**: 2025-10-18

## 研究任務清單

### 技術整合研究
- **Node.js + SQL Server 整合模式**: 研究 Sequelize ORM 與 SQL Server 的最佳實踐
- **Express.js 安全實作**: 研究 Session-based 認證與 JWT 的實作模式
- **第三方郵件服務整合**: 研究 SendGrid/Mailgun API 的錯誤處理與重試機制
- **速率限制實作**: 研究 IP-based 速率限制在 Express 中的實作方式

### 依賴套件研究
- **Sequelize ORM 最佳實踐**: 研究模型定義、關聯設定、遷移策略
- **密碼雜湊演算法**: 研究 bcrypt 配置與效能考量
- **JWT Token 管理**: 研究 token 過期、刷新機制
- **輸入驗證**: 研究 Joi 或 express-validator 的使用模式

### 架構模式研究
- **Express 應用架構**: 研究路由、控制器、服務的分層設計
- **錯誤處理模式**: 研究統一錯誤響應格式
- **日誌記錄**: 研究結構化日誌實作

---

## 研究結果與決策

### 技術整合決策

**決策**: 使用 Sequelize ORM 搭配 mssql 驅動進行 SQL Server 整合
**理由**: Sequelize 提供完整的 ORM 功能，支援模型定義、關聯、遷移。mssql 驅動穩定且效能良好，適合企業環境。
**替代方案考量**:
- Prisma: 更現代的 ORM，但學習曲線較陡，SQL Server 支援相對較新
- 直接使用 mssql 驅動: 需手動處理 SQL，增加開發複雜度
- TypeORM: 功能完整但設定較複雜

**決策**: 使用 express-session 搭配 connect-session-sequelize 實作 Session-based 認證
**理由**: 符合規格要求的安全 Session 管理，支援資料庫持久化，防止記憶體洩漏。
**替代方案考量**:
- JWT-only: 無法滿足 Session 要求，且難以處理登出失效
- Redis Session Store: 增加額外依賴，對於小型應用過於複雜

**決策**: 使用 Nodemailer 搭配 SendGrid/Mailgun 傳輸實作郵件服務
**理由**: Nodemailer 提供統一接口，支援多種傳輸方式，易於切換服務商。SendGrid/Mailgun 都有穩定 SDK。
**替代方案考量**:
- 直接使用服務商 SDK: 增加切換成本，程式碼重用性差
- 自建 SMTP: 需處理垃圾郵件問題，運維複雜度高

**決策**: 使用 express-rate-limit 中間件實作 IP-based 速率限制
**理由**: 輕量級、易配置，支援記憶體/Redis 儲存，符合小型應用需求。
**替代方案考量**:
- 自建速率限制: 增加開發時間，容易出錯
- Nginx 層級限制: 需額外配置，不利於開發環境測試

### 依賴套件決策

**決策**: 使用 bcryptjs (純 JavaScript 實作) 進行密碼雜湊
**理由**: 跨平台相容性好，效能足夠，無需編譯依賴。
**替代方案考量**:
- bcrypt (原生): 需要編譯，Windows 環境相容性問題
- Argon2: 更安全但複雜度較高，對於小型應用過於重量級

**決策**: 使用 jsonwebtoken 搭配 jwks-rsa 實作 JWT 管理
**理由**: 業界標準，支援非對稱加密，jwks-rsa 提供 RSA 金鑰管理。
**替代方案考量**:
- 自建 token 管理: 安全性風險高，重複造輪子
- 簡單對稱加密: 無法滿足企業級安全性要求

**決策**: 使用 Joi 進行輸入驗證
**理由**: 功能強大，錯誤訊息可自訂，支援複雜驗證規則，TypeScript 支援良好。
**替代方案考量**:
- express-validator: 基於 validator.js，學習曲線較平緩
- 自建驗證: 容易出錯，維護成本高

### 架構模式決策

**決策**: 採用分層架構 (Routes → Controllers → Services → Models)
**理由**: 關注點分離，易於測試和維護，每層有明確責任。
**替代方案考量**:
- 單一檔案: 難以維護，測試困難
- Repository 模式: 對於小型應用過於複雜

**決策**: 實作統一錯誤處理中間件，回傳一致 JSON 格式
**理由**: 提升 API 一致性，用戶體驗更好，錯誤追蹤更容易。
**替代方案考量**:
- 各端點自行處理: 容易不一致，維護困難
- 簡單 try-catch: 無法提供統一用戶體驗

**決策**: 使用 Winston 實作結構化日誌記錄
**理由**: 支援多種傳輸方式，結構化日誌易於搜尋和分析。
**替代方案考量**:
- Console.log: 無法滿足生產環境需求
- Morgan: 專注 HTTP 日誌，不夠全面

---

## 實作規範

### 程式碼風格
- 使用 ESLint + Airbnb 規則
- 強制使用 async/await，禁止 Promise 鏈式呼叫
- 所有函數必須有 JSDoc 註解
- 使用 PascalCase 命名類別，camelCase 命名函數和變數

### 安全性實作
- 所有用戶輸入必須通過 Joi 驗證
- 密碼永不記錄到日誌
- 使用 Helmet 中間件設定安全標頭
- 實作 CORS 策略
- Session cookie 設定 httpOnly 和 secure

### 錯誤處理
- 自訂錯誤類別 (ValidationError, AuthenticationError, etc.)
- 統一錯誤響應格式: `{ success: false, error: { code, message } }`
- 記錄詳細錯誤到日誌，但回傳用戶友善訊息

### 測試策略
- 單元測試覆蓋率 ≥80%
- API 合約測試使用 Supertest
- 整合測試包含資料庫操作
- 使用 testcontainers 進行資料庫測試隔離