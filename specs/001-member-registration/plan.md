# 實現計劃: 會員註冊流程

**分支**: `001-member-registration` | **日期**: 2025-10-18 | **規範**: /specs/001-member-registration/spec.md
**輸入**: 功能規範來自 `/specs/001-member-registration/spec.md`

**注意**: 本計劃根據 speckit.plan 工作流程生成，專注於後端 REST API 實現。

## 摘要

實現會員註冊流程的核心功能：用戶身份驗證、郵件驗證、雙重角色系統（一般用戶與管理員），以及安全防護機制。採用 Node.js + Express 架構，搭配 SQL Server 資料庫，提供完整的 REST API 服務。

## 技術背景

**語言/版本**: Node.js 18+ (LTS)
**主要依賴**: Express.js 4.x, Sequelize ORM, bcrypt, jsonwebtoken, nodemailer
**存儲**: SQL Server 2019+ (使用 mssql 驅動)
**測試**: Jest + Supertest (單元測試與 API 測試)
**目標平台**: Linux/macOS/Windows 伺服器環境
**專案類型**: Web API (後端服務)
**性能目標**: API 回應時間 ≤500ms (p95)，支援每分鐘 10 個並發請求
**約束**: 單一伺服器部署，簡單資料庫設計，第三方郵件服務 API 整合
**規模/範圍**: 小型應用 (每月 < 1,000 註冊)，雙重角色系統

## 憲章檢查

*閘門: 必須在第 0 階段研究前通過。第 1 階段設計後重新檢查。*

本功能必須滿足專案憲章中的所有五個核心原則:

- **代碼品質**: 實現必須遵守 ESLint 標準，展示清晰、易維護的 Express 架構，
  每個路由/服務具有單一責任，使用適當的 middleware 分層
- **測試標準**: TDD 強制 - 先編寫測試，關鍵路徑需要 ≥80% 涵蓋率，
  所有 API 端點必須有合約測試，使用 Supertest 進行整合測試
- **用戶體驗一致性**: 所有 API 回應必須遵循一致的 JSON 格式，錯誤訊息必須清晰且可行動，
  支援繁體中文錯誤訊息，可訪問性考量在 API 設計中體現
- **性能要求**: API ≤500ms p95 (調整為符合小型應用規模)、資料庫查詢最佳化、
  基本負載測試、CI 中監控性能指標
- **文檔本地化**: 所有規範、計劃與 API 文檔必須使用繁體中文 (zh-TW) 編寫，
  錯誤訊息以繁體中文提供

**驗證**: 代碼審查必須明確驗證每項原則的合規性。

**設計後重新評估**: ✅ 通過 - 設計完全符合憲章要求，分層架構確保可維護性，API 合約包含本地化錯誤訊息，性能目標符合小型應用規模。

## 專案結構

### 文檔 (本功能)

```
specs/001-member-registration/
├── plan.md              # This file (當前文件)
├── research.md          # Phase 0 output (已完成)
├── data-model.md        # Phase 1 output (已完成)
├── quickstart.md        # Phase 1 output (已完成)
├── contracts/           # Phase 1 output (已完成)
│   ├── registration-api.yaml
│   └── auth-api.yaml
├── tasks.md             # Phase 2 output (已完成)
```
```

### Source Code (repository root)

```
src/
├── config/              # 資料庫、郵件服務配置
│   ├── database.js
│   ├── email.js
│   └── index.js
├── controllers/         # API 控制器
│   ├── authController.js
│   ├── registrationController.js
│   └── userController.js
├── middleware/          # Express 中間件
│   ├── auth.js
│   ├── rateLimit.js
│   ├── validation.js
│   └── errorHandler.js
├── models/              # Sequelize 模型
│   ├── User.js
│   ├── VerificationCode.js
│   └── index.js
├── routes/              # API 路由定義
│   ├── auth.js
│   ├── registration.js
│   └── index.js
├── services/            # 業務邏輯服務
│   ├── emailService.js
│   ├── userService.js
│   └── authService.js
├── utils/               # 工具函數
│   ├── password.js
│   ├── validation.js
│   └── logger.js
├── app.js               # Express 應用設定
└── server.js            # 伺服器啟動腳本

tests/
├── unit/                # 單元測試
│   ├── services/
│   ├── middleware/
│   └── utils/
├── integration/         # 整合測試
│   ├── auth.test.js
│   ├── registration.test.js
│   └── rateLimit.test.js
└── contract/            # 合約測試
    ├── registration-api.test.js
    └── auth-api.test.js

scripts/                 # 部署與維護腳本
├── init-db.js
├── migrate.js
└── seed.js
```

**Structure Decision**: 採用單一 Express 專案結構，專注於後端 API 開發。使用分層架構 (routes → controllers → services → models) 確保關注點分離和可測試性。

## Complexity Tracking

*無憲章違規 - 所有實現符合小型應用規模和既有技術約束*