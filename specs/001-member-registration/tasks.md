# 任務: 會員註冊流程

**輸入**: 設計文檔來自 `/specs/001-member-registration/`
**先決條件**: plan.md (必需)、spec.md (用戶故事必需)、research.md、data-model.md、contracts/
**語言**: 所有文檔必須使用繁體中文 (zh-TW)

**測試**: 根據憲章要求，實作 TDD 方法，包含單元測試、合約測試和整合測試。

**組織**: 任務按用戶故事分組，以啟用每個故事的獨立實現和測試。

## 格式: `[ID] [P?] [Story] 描述`
- **[P]**: 可並行運行 (不同檔案，無依賴)
- **[Story]**: 此任務屬於哪個用戶故事 (例如: US1、US2、US3)
- 在描述中包含精確的檔案路徑

## 路徑慣例
- **專案結構**: 存放庫根目錄的 `src/`、`tests/`
- 根據 plan.md 定義的分層架構：routes → controllers → services → models

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Node.js + Express 專案初始化和基本結構設定

- [ ] T001 建立專案目錄結構 per implementation plan
- [ ] T002 初始化 Node.js 專案 with package.json 和基礎依賴
- [ ] T003 [P] 配置 ESLint 和 Prettier 代碼品質工具
- [ ] T004 [P] 設置 Jest 測試框架和覆蓋率報告
- [ ] T005 [P] 配置環境變數管理 (.env 支援)
- [ ] T006 [P] 驗證所有文檔與模板使用繁體中文 (zh-TW)

---

## Phase 2: Foundational (Blocking Prerequisites)

**目的**: 任何用戶故事實現前必須完成的 Node.js + SQL Server 核心基礎設施

**⚠️ 關鍵**: 此階段完成前無法開始任何用戶故事工作

- [ ] T007 設置 Sequelize ORM 與 SQL Server 連線配置
- [ ] T008 [P] 實現 express-session 中間件與 Sequelize Session Store
- [ ] T009 [P] 建立 Express 應用程式基本架構 (app.js, server.js)
- [ ] T010 [P] 配置統一錯誤處理中間件 (errorHandler.js)
- [ ] T011 [P] 設置 Winston 日誌記錄基礎設施
- [ ] T012 [P] 實作 express-rate-limit IP 速率限制中間件
- [ ] T013 [P] 建立 Joi 輸入驗證工具函數
- [ ] T014 [P] 配置 Nodemailer 與 SendGrid/Mailgun 郵件服務
- [ ] T015 設置資料庫遷移腳本和初始化資料
- [ ] T016 建立基礎測試輔助函數和資料庫測試隔離

**檢查點**: 基礎設施就緒 - 用戶故事實現現可並行開始

---

## Phase 3: 用戶故事 1 - 完成基本註冊 (優先級: P1) 🎯 MVP

**目標**: 用戶能夠填寫個人資料並設定密碼，成功建立帳號

**獨立測試**: 通過檢查資料庫中是否建立新用戶記錄，並驗證用戶能以設定密碼登入來完全測試

### 用戶故事 1 的品質與測試任務 (憲章要求)

- [ ] T017 [P] [US1] 為 User 模型編寫單位測試 (首先) 在 tests/unit/models/user.test.js
- [ ] T018 [P] [US1] 為註冊端點編寫合約測試 (首先) 在 tests/contract/registration-api.test.js
- [ ] T019 [P] [US1] 為註冊用戶旅程編寫整合測試 在 tests/integration/registration-flow.test.js
- [ ] T020 [US1] 為註冊關鍵路徑新增性能基準 (PERF-001)

### 用戶故事 1 的實現任務

- [ ] T021 [P] [US1] 在 src/models/User.js 建立 User Sequelize 模型
- [ ] T022 [P] [US1] 在 src/utils/validation.js 實作身分證格式驗證
- [ ] T023 [P] [US1] 在 src/utils/password.js 實作密碼雜湊與驗證
- [ ] T024 [US1] 在 src/services/userService.js 實現用戶註冊業務邏輯
- [ ] T025 [US1] 在 src/controllers/registrationController.js 實現註冊控制器
- [ ] T026 [US1] 在 src/routes/registration.js 定義註冊 API 路由
- [ ] T027 [US1] 在 src/middleware/validation.js 新增註冊資料驗證中間件
- [ ] T028 [US1] 按 UX 一致性原則新增註冊錯誤處理 (清晰的繁體中文錯誤訊息)
- [ ] T029 [US1] 為註冊操作新增結構化日誌記錄
- [ ] T030 [US1] UX 審查 - 驗證 API 回應格式與錯誤訊息的用戶友好性

**檢查點**: 此時用戶故事 1 應完全功能性且獨立可測試

---

## Phase 4: 用戶故事 2 - 完成 E-Mail 驗證 (優先級: P1)

**目標**: 註冊後的用戶能夠接收驗證郵件並輸入驗證碼來啟用完整帳號功能

**獨立測試**: 通過檢查驗證郵件是否寄出、驗證碼是否正確驗證帳號狀態來完全測試

### 用戶故事 2 的品質與測試任務 (憲章要求)

- [ ] T031 [P] [US2] 為 VerificationCode 模型編寫單位測試 (首先) 在 tests/unit/models/verificationCode.test.js
- [ ] T032 [P] [US2] 為驗證端點編寫合約測試 (首先) 在 tests/contract/verification-api.test.js
- [ ] T033 [P] [US2] 為郵件驗證用戶旅程編寫整合測試 在 tests/integration/email-verification-flow.test.js
- [ ] T034 [US2] 為驗證關鍵路徑新增性能基準 (PERF-001)

### 用戶故事 2 的實現任務

- [ ] T035 [P] [US2] 在 src/models/VerificationCode.js 建立 VerificationCode Sequelize 模型
- [ ] T036 [P] [US2] 在 src/services/emailService.js 實現郵件發送服務
- [ ] T037 [P] [US2] 在 src/utils/verification.js 實作驗證碼生成與雜湊
- [ ] T038 [US2] 在 src/services/verificationService.js 實現驗證業務邏輯
- [ ] T039 [US2] 在 src/controllers/registrationController.js 新增驗證控制器方法
- [ ] T040 [US2] 在 src/routes/registration.js 新增驗證 API 路由 (/verify, /resend)
- [ ] T041 [US2] 在 src/middleware/validation.js 新增驗證資料驗證中間件
- [ ] T042 [US2] 實作驗證碼過期檢查與清理機制
- [ ] T043 [US2] 按 UX 一致性原則新增驗證錯誤處理 (繁體中文錯誤訊息)
- [ ] T044 [US2] 為驗證操作新增詳細日誌記錄
- [ ] T045 [US2] UX 審查 - 驗證驗證流程的用戶體驗一致性

**檢查點**: 此時用戶故事 2 應完全功能性且獨立可測試

---

## Phase 5: 用戶故事 3 - 使用未驗證帳號登入 (優先級: P2)

**目標**: 未完成 E-Mail 驗證的用戶仍能登入但功能受限

**獨立測試**: 通過未驗證用戶登入後檢查功能可用性來完全測試

### 用戶故事 3 的品質與測試任務 (憲章要求)

- [ ] T046 [P] [US3] 為 Session 模型編寫單位測試 (首先) 在 tests/unit/models/session.test.js
- [ ] T047 [P] [US3] 為認證端點編寫合約測試 (首先) 在 tests/contract/auth-api.test.js
- [ ] T048 [P] [US3] 為登入用戶旅程編寫整合測試 在 tests/integration/login-flow.test.js
- [ ] T049 [US3] 為登入關鍵路徑新增性能基準 (PERF-001)

### 用戶故事 3 的實現任務

- [ ] T050 [P] [US3] 在 src/models/Session.js 建立 Session Sequelize 模型
- [ ] T051 [P] [US3] 在 src/services/authService.js 實現認證業務邏輯
- [ ] T052 [P] [US3] 在 src/controllers/authController.js 實現認證控制器
- [ ] T053 [US3] 在 src/routes/auth.js 定義認證 API 路由 (/login, /logout, /me)
- [ ] T054 [US3] 在 src/middleware/auth.js 實作 Session 認證中間件
- [ ] T055 [US3] 實作未驗證用戶功能限制檢查
- [ ] T056 [US3] 在 src/controllers/userController.js 實現用戶資訊控制器
- [ ] T057 [US3] 按 UX 一致性原則新增認證錯誤處理 (繁體中文錯誤訊息)
- [ ] T058 [US3] 為認證操作新增安全日誌記錄
- [ ] T059 [US3] UX 審查 - 驗證登入流程與功能限制的用戶體驗

**檢查點**: 此時用戶故事 3 應完全功能性且獨立可測試

---

## Final Phase: Polish & Cross-Cutting Concerns

**目的**: 完成所有用戶故事後的整體優化和跨切面關注點

- [ ] T060 實作角色管理功能 (管理員指定機制)
- [ ] T061 [P] 新增 API 文檔生成 (Swagger/OpenAPI)
- [ ] T062 [P] 實作健康檢查端點 (/health)
- [ ] T063 [P] 配置 CORS 中間件
- [ ] T064 [P] 新增請求記錄中間件
- [ ] T065 實作應用程式啟動檢查 (資料庫連線, 外部服務)
- [ ] T066 [P] 建立 Docker 容器化配置
- [ ] T067 [P] 配置 PM2 程序管理
- [ ] T068 執行完整整合測試套件
- [ ] T069 執行負載測試驗證性能基準
- [ ] T070 最終安全審查與漏洞掃描
- [ ] T071 文檔本地化最終檢查 - 確保所有用戶文檔使用繁體中文
- [ ] T072 建立生產部署指南

---

## 依賴關係圖

```
用戶故事完成順序:
US1 (基本註冊) → US2 (郵件驗證) → US3 (登入)

並行執行機會:
- Phase 1 & 2: 可完全並行
- US1 內部: 模型與服務可並行開發
- US2 內部: 郵件服務與驗證邏輯可並行
- US3 內部: Session 管理與認證中間件可並行

跨故事依賴:
- US2 依賴 US1 的 User 模型
- US3 依賴 US1 的 User 模型和 US2 的驗證狀態
```

## 並行執行範例

**開發者 A**: Phase 1 (Setup) + US1 模型與測試
**開發者 B**: Phase 2 (Foundational) + US2 郵件服務
**開發者 C**: US3 認證系統

## 實作策略

**MVP 範圍**: US1 (基本註冊) - 提供核心註冊功能
**增量交付**: US1 → US1+US2 → US1+US2+US3
**品質閘門**: 每個階段結束時執行完整測試套件
**回滾計劃**: 資料庫遷移支援回滾，功能標記支援功能切換

---

## 成功標準

- **功能完整性**: 所有用戶故事的接納標準通過
- **測試覆蓋率**: ≥80% 關鍵路徑覆蓋
- **性能基準**: API ≤500ms p95，支援 10 req/min
- **安全標準**: 通過基本安全審查
- **文檔完整性**: 所有用戶文檔使用繁體中文