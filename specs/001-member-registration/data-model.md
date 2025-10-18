# 資料模型設計

**功能**: 會員註冊流程 | **階段**: 1 (設計) | **日期**: 2025-10-18

## 實體定義

### 用戶帳號 (User)

**用途**: 代表註冊用戶，儲存基本資訊和驗證狀態

**欄位定義**:
```javascript
{
  id: {
    type: Sequelize.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  nationalId: {
    type: Sequelize.STRING(10),
    allowNull: false,
    unique: true,
    comment: '身分證字號'
  },
  name: {
    type: Sequelize.STRING(50),
    allowNull: false,
    comment: '姓名'
  },
  email: {
    type: Sequelize.STRING(255),
    allowNull: false,
    unique: true,
    comment: '電子郵件'
  },
  passwordHash: {
    type: Sequelize.STRING(255),
    allowNull: false,
    comment: '密碼雜湊'
  },
  role: {
    type: Sequelize.ENUM('user', 'admin'),
    allowNull: false,
    defaultValue: 'user',
    comment: '用戶角色'
  },
  isVerified: {
    type: Sequelize.BOOLEAN,
    allowNull: false,
    defaultValue: false,
    comment: '是否已驗證'
  },
  sessionId: {
    type: Sequelize.STRING(255),
    allowNull: true,
    comment: '當前 Session ID'
  },
  createdAt: {
    type: Sequelize.DATE,
    allowNull: false
  },
  updatedAt: {
    type: Sequelize.DATE,
    allowNull: false
  }
}
```

**索引**:
- PRIMARY KEY: id
- UNIQUE: nationalId, email
- INDEX: sessionId (用於 Session 查詢)

**驗證規則**:
- nationalId: 台灣身分證格式驗證 (A123456789)
- name: 2-50 字元，不可包含特殊符號
- email: 有效 email 格式
- passwordHash: bcrypt 雜湊，長度限制
- role: 只能是 'user' 或 'admin'

---

### 驗證記錄 (VerificationCode)

**用途**: 追蹤發送的驗證碼，確保一次性使用和時效性

**欄位定義**:
```javascript
{
  id: {
    type: Sequelize.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  userId: {
    type: Sequelize.INTEGER,
    allowNull: false,
    references: {
      model: 'Users',
      key: 'id'
    },
    comment: '關聯用戶 ID'
  },
  code: {
    type: Sequelize.STRING(6),
    allowNull: false,
    comment: '6 位數驗證碼'
  },
  hashedCode: {
    type: Sequelize.STRING(255),
    allowNull: false,
    comment: '驗證碼雜湊 (安全性)'
  },
  email: {
    type: Sequelize.STRING(255),
    allowNull: false,
    comment: '發送目標郵件'
  },
  isUsed: {
    type: Sequelize.BOOLEAN,
    allowNull: false,
    defaultValue: false,
    comment: '是否已被使用'
  },
  expiresAt: {
    type: Sequelize.DATE,
    allowNull: false,
    comment: '過期時間'
  },
  createdAt: {
    type: Sequelize.DATE,
    allowNull: false
  }
}
```

**索引**:
- PRIMARY KEY: id
- FOREIGN KEY: userId → Users.id
- INDEX: email, createdAt (用於清理過期記錄)
- INDEX: hashedCode (用於驗證查詢)

**驗證規則**:
- code: 必須是 6 位數字
- hashedCode: bcrypt 雜湊，用於安全比較
- expiresAt: 必須是未來時間，預設 5 分鐘後

---

### Session 記錄 (Session - Sequelize Session Store)

**用途**: 儲存用戶 Session 資料，支援 Session-based 認證

**欄位定義**:
```javascript
{
  sid: {
    type: Sequelize.STRING(255),
    primaryKey: true,
    comment: 'Session ID'
  },
  userId: {
    type: Sequelize.INTEGER,
    allowNull: false,
    references: {
      model: 'Users',
      key: 'id'
    },
    comment: '關聯用戶 ID'
  },
  data: {
    type: Sequelize.TEXT,
    allowNull: false,
    comment: 'Session 資料 (JSON)'
  },
  expiresAt: {
    type: Sequelize.DATE,
    allowNull: false,
    comment: '過期時間'
  },
  createdAt: {
    type: Sequelize.DATE,
    allowNull: false
  },
  updatedAt: {
    type: Sequelize.DATE,
    allowNull: false
  }
}
```

**索引**:
- PRIMARY KEY: sid
- FOREIGN KEY: userId → Users.id
- INDEX: expiresAt (用於清理過期 Session)

---

## 實體關聯

```
Users (1) ──── (N) VerificationCodes
  │
  └── (1) ──── (1) Sessions (一對一，但 Session 可為空)
```

**關聯說明**:
- User → VerificationCode: 一對多，用戶可以有多個驗證記錄 (重發情況)
- User → Session: 一對一，用戶同時只能有一個活躍 Session

---

## 資料庫遷移策略

### 初始遷移 (001-create-users.js)
```javascript
module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.createTable('Users', {
      // User 模型欄位定義
    });
  },
  down: async (queryInterface, Sequelize) => {
    await queryInterface.dropTable('Users');
  }
};
```

### 驗證碼遷移 (002-create-verification-codes.js)
```javascript
module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.createTable('VerificationCodes', {
      // VerificationCode 模型欄位定義
    });
    // 新增外鍵約束
    await queryInterface.addConstraint('VerificationCodes', {
      fields: ['userId'],
      type: 'foreign key',
      references: {
        table: 'Users',
        field: 'id'
      },
      onDelete: 'CASCADE'
    });
  },
  down: async (queryInterface, Sequelize) => {
    await queryInterface.dropTable('VerificationCodes');
  }
};
```

### Session 遷移 (003-create-sessions.js)
```javascript
module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.createTable('Sessions', {
      // Session 模型欄位定義
    });
  },
  down: async (queryInterface, Sequelize) => {
    await queryInterface.dropTable('Sessions');
  }
};
```

---

## 資料完整性規則

### 唯一約束
- Users.nationalId: 確保身分證唯一性
- Users.email: 確保郵件唯一性
- Sessions.sid: Session ID 唯一性

### 外鍵約束
- VerificationCodes.userId → Users.id (CASCADE DELETE)
- Sessions.userId → Users.id (CASCADE DELETE)

### 業務規則驗證
- 用戶註冊時檢查 nationalId 和 email 唯一性
- 驗證碼使用後標記為已使用，防止重複使用
- Session 過期自動清理
- 管理員角色只能由系統指定，不能通過一般註冊取得

---

## 效能考量

### 索引策略
- Users.nationalId, Users.email: 唯一索引，支援快速查詢
- VerificationCodes.hashedCode: 支援驗證碼比對
- Sessions.expiresAt: 支援過期 Session 清理

### 查詢優化
- 使用 Sequelize include/eager loading 減少 N+1 查詢
- 複雜查詢使用原始 SQL 優化
- 定期清理過期驗證碼和 Session 記錄

### 快取策略
- 對於小型應用，主要依賴資料庫索引
- Session 資料可考慮 Redis 快取 (未來擴展時)