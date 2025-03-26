---
title: 'マーメイド記法を学ぶゾウ'
emoji: '🐘'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: [postgreSQL]
published: false
---

# 概要・結果

```mermaid
erDiagram
Staff {
  int id PK
  text name
  text icon
  text dark_mode
  enum role
  datetime created_at
  datetime updated_at
}

Permission {
  int id PK
  text name
  text description
  datetime created_at
  datetime updated_at
}

RolePermission {
  int id PK
  enum role
  string permission_id
  datetime created_at
  datetime updated_at
}

ServiceOfficePermission {
  int id PK
  int staff_id FK
  int service_office_id FK
  int permission_id FK
  int granted_by FK
  datetime created_at
  datetime updated_at
}

    Message {
        int id PK
        int sender_id FK
        int recipient_id FK
        text text
        boolean read
        datetime sent_at
        datetime created_at
        datetime updated_at
    }

    ServiceRecipient {
        int id PK
        int service_office_id FK
        int staff_id FK
        text name
        datetime birthday
        text disease
        int created_by FK
        int last_modified_by FK
        datetime created_at
        datetime updated_at
    }


    StaffServiceOffice {
        int id PK
        int staff_id FK
        int service_office_id FK
        enum role
        boolean is_primary
        datetime created_at
        datetime updated_at
    }

    ServiceRecipientOffice {
        int id PK
        int service_recipient_id FK
        int service_office_id FK
        datetime created_at
        datetime updated_at
    }

    ServiceOffice {
        int id PK
        text name
        text type
        int created_by FK
        int last_modified_by FK
        datetime created_at
        datetime updated_at
    }

    Notice {
        int id PK
        int service_office_id FK
        int service_recipient_id FK
        int support_plan_id FK
        text message
        enum notification_type
        datetime deadline_date
        boolean is_read
        int priority
        datetime created_at
        datetime updated_at
    }

    StaffNotificationSettings {
        int id PK
        int staff_id FK
        boolean email_notifications
        int deadline_advance_days
        datetime created_at
        datetime updated_at
    }

    SupportPlanner {
        int id PK
        int service_recipient_id FK
        text name
        text contact
        datetime created_at
        datetime updated_at
    }

    SupportPlan {
        int id PK
        int service_recipient_id FK
        datetime planning_start
        datetime plan_deadlines
        boolean assessment
        boolean draft_plan
        boolean meeting_of_managers
        boolean this_plan
        boolean monitoring
        datetime created_at
        datetime updated_at
    }

    SupportPlanStatus {
        int id PK
        int support_plan_id FK
        enum step_type
        boolean completed
        datetime completed_at
        int completed_by FK
        text notes
        datetime created_at
        datetime updated_at
    }

    Deliverables {
        int id PK
        int support_plan_id FK
        text file_path
        text file_view
        datetime created_at
        datetime updated_at
    }

    GoogleIntegration {
      int id PK
      int staff_id FK
      text google_account
      text access_token
      text refresh_token
      datetime token_expiry
      datetime created_at
      datetime updated_at
    }

    GoogleDocument {
      int id
      int service_recipient_id FK
      int support_plan_id FK
      enum document_type
      text google_file_id
      text google_file_url
      datetime last_synced
      datetime created_at
      datetime updated_at
    }

    Staff ||--o{ Message : "sends"
    Staff ||--o{ StaffNotificationSettings : "configures"
    Staff ||--o{ StaffServiceOffice : "belongs to"
    Staff ||--o{ SupportPlanStatus : "completes"
    Staff ||--o{ GoogleIntegration : "google account"

    ServiceOffice ||--o{ StaffServiceOffice : "employs"
    ServiceOffice ||--o{ ServiceRecipient : "manages"
    ServiceOffice ||--o{ ServiceRecipientOffice : "connects with"
    ServiceOffice ||--o{ Notice : "generates"

    ServiceRecipient ||--o{ ServiceRecipientOffice : "belongs to"
    ServiceRecipient ||--o{ SupportPlanner : "has"
    ServiceRecipient ||--o{ SupportPlan : "requires"
    ServiceRecipient ||--o{ Notice : "receives"
    ServiceRecipient ||--o{ GoogleDocument : "support plan for staffs"

    SupportPlan ||--o{ SupportPlanStatus : "tracks progress with"
    SupportPlan ||--o{ Deliverables : "produces"
    SupportPlan ||--o{ Notice : "triggers"
    SupportPlan ||--o{ GoogleDocument : "triggers"

    // Permissionに関連するリレーションを追加
Permission ||--o{ RolePermission : "defines"
Permission ||--o{ ServiceOfficePermission : "grants"

// RolePermissionのリレーション
Staff ||--o{ RolePermission : "has role-based"
RolePermission }o--|| Permission : "associates"

// ServiceOfficePermissionのリレーション
Staff ||--o{ ServiceOfficePermission : "has office-specific"
ServiceOffice ||--o{ ServiceOfficePermission : "manages"
Staff ||--o{ ServiceOfficePermission : "grants permissions" // granted_byの関係

// ServiceOfficeの監査関連
Staff ||--o{ ServiceOffice : "creates"  // created_byの関係
Staff ||--o{ ServiceOffice : "modifies" // last_modified_byの関係

// ServiceRecipientの監査関連
Staff ||--o{ ServiceRecipient : "creates"  // created_byの関係
Staff ||--o{ ServiceRecipient : "modifies" // last_modified_byの関係
```
