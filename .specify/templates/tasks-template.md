---
description: "功能實現的任務清單模板"
---

# 任務: [功能名稱]

**輸入**: 設計文檔來自 `/specs/[###-feature-name]/`
**先決條件**: plan.md (必需)、spec.md (用戶故事必需)、research.md、data-model.md、contracts/
**語言**: 所有文檔必須使用繁體中文 (zh-TW)

**測試**: 下面的示例包含測試任務。測試是可選的 - 僅在功能規範中明確要求時包含。

**組織**: 任務按用戶故事分組，以啟用每個故事的獨立實現和測試。

## 格式: `[ID] [P?] [Story] 描述`
- **[P]**: 可並行運行 (不同檔案，無依賴)
- **[Story]**: 此任務屬於哪個用戶故事 (例如: US1、US2、US3)
- 在描述中包含精確的檔案路徑

## 路徑慣例
- **單一專案**: 存放庫根目錄的 `src/`、`tests/`
- **Web 應用**: `backend/src/`、`frontend/src/`
- **移動應用**: `api/src/`、`ios/src/` 或 `android/src/`
- 下面顯示的路徑假設單一專案 - 根據 plan.md 調整

<!-- 
  ============================================================================
  重要: 下面的任務是示例任務，僅供說明目的。
  
  /speckit.tasks 命令必須根據以下內容將這些替換為實際任務:
  - spec.md 中的用戶故事 (具有優先級 P1、P2、P3...)

  - Feature requirements from plan.md
  - Entities from data-model.md
  - Endpoints from contracts/
  
  Tasks MUST be organized by user story so each story can be:
  - Implemented independently
  - Tested independently
  - Delivered as an MVP increment
  
  DO NOT keep these sample tasks in the generated tasks.md file.
  ============================================================================
-->

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and basic structure

- [ ] T001 Create project structure per implementation plan
- [ ] T002 Initialize [language] project with [framework] dependencies
- [ ] T003 [P] Configure linting and formatting tools
- [ ] T004 [P] Setup code quality enforcement (ESLint, Prettier, Black, etc.)
- [ ] T005 [P] Configure test runner and coverage reporting
- [ ] T006 [P] Setup performance profiling and monitoring tools
- [ ] T006b [P] 驗證所有文檔與模板使用繁體中文 (zh-TW) - 包括 README、規範、計劃

---

## 第 2 階段: 基礎設施 (阻止性先決條件)

**目的**: 任何用戶故事實現前必須完成的核心基礎設施

**⚠️ 關鍵**: 此階段完成前無法開始任何用戶故事工作

基礎設施任務範例 (根據您的專案調整):

- [ ] T007 設置資料庫架構與遷移框架
- [ ] T008 [P] 實現認證/授權框架
- [ ] T009 [P] 設置 API 路由與中介軟體結構
- [ ] T010 建立所有故事依賴的基礎模型/實體
- [ ] T011 配置錯誤處理與日誌基礎設施
- [ ] T012 設置環境配置管理
- [ ] T013 [P] 建立基礎錯誤回應類型 (按 UX 一致性原則)
- [ ] T014 [P] 建立設計系統元件庫
- [ ] T015 設置 CI/CD 管道與品質閘門 (測試、涵蓋率、linting)
- [ ] T015b 設置文檔本地化檢查 - CI 應驗證規範/計劃/文檔均使用繁體中文

**檢查點**: 基礎設施就緒 - 用戶故事實現現可並行開始

---

## 第 3 階段: 用戶故事 1 - [標題] (優先級: P1) 🎯 MVP

**目標**: [簡短描述此故事提供的內容]

**獨立測試**: [如何驗證此故事獨立運作]

### 用戶故事 1 的品質與測試任務 (憲章要求)

- [ ] T016 [P] [US1] 為 [元件] 編寫單位測試 (首先) 在 tests/unit/test_[name].py
- [ ] T017 [P] [US1] 為 [端點] 編寫合約測試 (首先) 在 tests/contract/test_[name].py
- [ ] T018 [P] [US1] 為 [用戶旅程] 編寫整合測試 在 tests/integration/test_[name].py
- [ ] T019 [US1] 為 [關鍵路徑] 新增性能基準 (如適用 PERF-001/PERF-002)

### 用戶故事 1 的實現任務

- [ ] T020 [P] [US1] 在 src/models/[entity1].py 建立 [Entity1] 模型
- [ ] T021 [P] [US1] 在 src/models/[entity2].py 建立 [Entity2] 模型
- [ ] T022 [US1] 在 src/services/[service].py 實現 [Service] (取決於 T020、T021)
- [ ] T023 [US1] 在 src/[location]/[file].py 實現 [端點/功能]
- [ ] T024 [US1] 按 UX 一致性原則新增驗證與錯誤處理 (清晰的錯誤訊息)
- [ ] T025 [US1] 為用戶故事 1 操作新增日誌記錄
- [ ] T026 [US1] UX 審查 - 驗證對設計系統與可訪問性標準的對齐
- [ ] T026b [US1] 文檔本地化審查 - 確認所有規範、計劃與文檔使用繁體中文

**檢查點**: 此時用戶故事 1 應完全功能性且獨立可測試

---

## 第 4 階段: 用戶故事 2 - [標題] (優先級: P2)

**目標**: [簡短描述此故事提供的內容]

**獨立測試**: [如何驗證此故事獨立運作]

### 用戶故事 2 的品質與測試任務 (憲章要求)

- [ ] T027 [P] [US2] 為 [元件] 編寫單位測試 (首先) 在 tests/unit/test_[name].py
- [ ] T028 [P] [US2] 為 [端點] 編寫合約測試 (首先) 在 tests/contract/test_[name].py
- [ ] T029 [US2] 如適用新增性能基準

### 用戶故事 2 的實現任務

- [ ] T030 [P] [US2] 在 src/models/[entity].py 建立 [Entity] 模型
- [ ] T031 [US2] 在 src/services/[service].py 實現 [Service]
- [ ] T032 [US2] 在 src/[location]/[file].py 實現 [端點/功能]
- [ ] T033 [US2] 新增驗證與錯誤處理
- [ ] T034 [US2] UX 審查 - 驗證設計系統合規性
- [ ] T034b [US2] 文檔本地化審查 - 繁體中文驗證

### Quality & Testing Tasks for User Story 1 (REQUIRED per Constitution)

- [ ] T016 [P] [US1] Write unit tests (FIRST) for [component] in tests/unit/test_[name].py
- [ ] T017 [P] [US1] Write contract tests (FIRST) for [endpoint] in tests/contract/test_[name].py
- [ ] T018 [P] [US1] Write integration tests for [user journey] in tests/integration/test_[name].py
- [ ] T019 [US1] Add performance benchmarks for [critical path] if PERF-001/PERF-002 apply

### Implementation Tasks for User Story 1

- [ ] T020 [P] [US1] Create [Entity1] model in src/models/[entity1].py
- [ ] T021 [P] [US1] Create [Entity2] model in src/models/[entity2].py
- [ ] T022 [US1] Implement [Service] in src/services/[service].py (depends on T020, T021)
- [ ] T023 [US1] Implement [endpoint/feature] in src/[location]/[file].py
- [ ] T024 [US1] Add validation and error handling per UX consistency principle (clear error messages)
- [ ] T025 [US1] Add logging for user story 1 operations
- [ ] T026 [US1] UX review - verify alignment with design system and accessibility standards

**Checkpoint**: At this point, User Story 1 should be fully functional and testable independently

---

## Phase 4: User Story 2 - [Title] (Priority: P2)

**Goal**: [Brief description of what this story delivers]

**Independent Test**: [How to verify this story works on its own]

### Quality & Testing Tasks for User Story 2 (REQUIRED per Constitution)

- [ ] T027 [P] [US2] Write unit tests (FIRST) for [component] in tests/unit/test_[name].py
- [ ] T028 [P] [US2] Write contract tests (FIRST) for [endpoint] in tests/contract/test_[name].py
- [ ] T029 [US2] Add performance benchmarks if applicable

### Implementation Tasks for User Story 2

- [ ] T030 [P] [US2] Create [Entity] model in src/models/[entity].py
- [ ] T031 [US2] Implement [Service] in src/services/[service].py
- [ ] T032 [US2] Implement [endpoint/feature] in src/[location]/[file].py
- [ ] T033 [US2] Add validation and error handling
- [ ] T034 [US2] UX review - verify design system compliance
- [ ] T019 [P] [US2] Integration test for [user journey] in tests/integration/test_[name].py

### Implementation for User Story 2

- [ ] T020 [P] [US2] Create [Entity] model in src/models/[entity].py
- [ ] T021 [US2] Implement [Service] in src/services/[service].py
- [ ] T022 [US2] Implement [endpoint/feature] in src/[location]/[file].py
- [ ] T023 [US2] Integrate with User Story 1 components (if needed)

**Checkpoint**: At this point, User Stories 1 AND 2 should both work independently

---

## Phase 5: User Story 3 - [Title] (Priority: P3)

**Goal**: [Brief description of what this story delivers]

**Independent Test**: [How to verify this story works on its own]

### Tests for User Story 3 (OPTIONAL - only if tests requested) ⚠️

- [ ] T024 [P] [US3] Contract test for [endpoint] in tests/contract/test_[name].py
- [ ] T025 [P] [US3] Integration test for [user journey] in tests/integration/test_[name].py

### Implementation for User Story 3

- [ ] T026 [P] [US3] Create [Entity] model in src/models/[entity].py
- [ ] T027 [US3] Implement [Service] in src/services/[service].py
- [ ] T028 [US3] Implement [endpoint/feature] in src/[location]/[file].py

**Checkpoint**: All user stories should now be independently functional

---

[Add more user story phases as needed, following the same pattern]

---

## Phase N: Polish & Cross-Cutting Concerns

**Purpose**: Improvements that affect multiple user stories

- [ ] TXXX [P] Documentation updates in docs/
- [ ] TXXX Code cleanup and refactoring
- [ ] TXXX Performance optimization across all stories
- [ ] TXXX [P] Additional unit tests (if requested) in tests/unit/
- [ ] TXXX Security hardening
- [ ] TXXX Run quickstart.md validation

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Stories (Phase 3+)**: All depend on Foundational phase completion
  - User stories can then proceed in parallel (if staffed)
  - Or sequentially in priority order (P1 → P2 → P3)
- **Polish (Final Phase)**: Depends on all desired user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Foundational (Phase 2) - No dependencies on other stories
- **User Story 2 (P2)**: Can start after Foundational (Phase 2) - May integrate with US1 but should be independently testable
- **User Story 3 (P3)**: Can start after Foundational (Phase 2) - May integrate with US1/US2 but should be independently testable

### Within Each User Story

- Tests (if included) MUST be written and FAIL before implementation
- Models before services
- Services before endpoints
- Core implementation before integration
- Story complete before moving to next priority

### Parallel Opportunities

- All Setup tasks marked [P] can run in parallel
- All Foundational tasks marked [P] can run in parallel (within Phase 2)
- Once Foundational phase completes, all user stories can start in parallel (if team capacity allows)
- All tests for a user story marked [P] can run in parallel
- Models within a story marked [P] can run in parallel
- Different user stories can be worked on in parallel by different team members

---

## Parallel Example: User Story 1

```bash
# Launch all tests for User Story 1 together (if tests requested):
Task: "Contract test for [endpoint] in tests/contract/test_[name].py"
Task: "Integration test for [user journey] in tests/integration/test_[name].py"

# Launch all models for User Story 1 together:
Task: "Create [Entity1] model in src/models/[entity1].py"
Task: "Create [Entity2] model in src/models/[entity2].py"
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup
2. Complete Phase 2: Foundational (CRITICAL - blocks all stories)
3. Complete Phase 3: User Story 1
4. **STOP and VALIDATE**: Test User Story 1 independently
5. Deploy/demo if ready

### Incremental Delivery

1. Complete Setup + Foundational → Foundation ready
2. Add User Story 1 → Test independently → Deploy/Demo (MVP!)
3. Add User Story 2 → Test independently → Deploy/Demo
4. Add User Story 3 → Test independently → Deploy/Demo
5. Each story adds value without breaking previous stories

### Parallel Team Strategy

With multiple developers:

1. Team completes Setup + Foundational together
2. Once Foundational is done:
   - Developer A: User Story 1
   - Developer B: User Story 2
   - Developer C: User Story 3
3. Stories complete and integrate independently

---

## Notes

- [P] tasks = different files, no dependencies
- [Story] label maps task to specific user story for traceability
- Each user story should be independently completable and testable
- Verify tests fail before implementing
- Commit after each task or logical group
- Stop at any checkpoint to validate story independently
- Avoid: vague tasks, same file conflicts, cross-story dependencies that break independence



