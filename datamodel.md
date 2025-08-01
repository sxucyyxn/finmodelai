# FinModel AI – Data Model

---

## 🧑‍💼 Users

| Field              | Type        | Description                            |
|-------------------|-------------|----------------------------------------|
| user_id           | UUID        | Primary Key                            |
| name              | String      | Full name                              |
| email             | String      | Unique user email                      |
| password_hash     | String      | Hashed password                        |
| organization_id   | UUID        | Foreign key to Organizations           |
| role              | Enum        | Admin, Analyst, Viewer                 |
| created_at        | Timestamp   | Registration timestamp                 |
| last_login        | Timestamp   | Last login timestamp                   |

---

## 🏢 Organizations

| Field              | Type        | Description                            |
|-------------------|-------------|----------------------------------------|
| organization_id   | UUID        | Primary Key                            |
| name              | String      | Company or team name                   |
| industry          | String      | Optional industry tag                  |
| created_at        | Timestamp   | Date of creation                       |

---

## 📦 Models

| Field              | Type        | Description                            |
|-------------------|-------------|----------------------------------------|
| model_id          | UUID        | Primary Key                            |
| user_id           | UUID        | Owner/creator of the model             |
| organization_id   | UUID        | Related organization                   |
| model_name        | String      | Descriptive name                       |
| model_type        | Enum        | SaaS, E-commerce, VC, PE, LBO, etc.    |
| version           | Integer     | Version number                         |
| status            | Enum        | Draft, Final, Shared                   |
| created_at        | Timestamp   | Date of creation                       |
| last_updated      | Timestamp   | Last modified date                     |

---

## 🧾 Assumptions

| Field              | Type        | Description                            |
|-------------------|-------------|----------------------------------------|
| assumption_id     | UUID        | Primary Key                            |
| model_id          | UUID        | Foreign key to Models                  |
| category          | Enum        | Revenue, COGS, Headcount, CAC, etc.    |
| label             | String      | Human-readable assumption label        |
| value             | Float/Text  | Assumption value or formula            |
| unit              | String      | %, $, #, etc.                          |
| editable          | Boolean     | Whether user can change it             |
| created_at        | Timestamp   | Creation time                          |

---

## 📊 Metrics & KPIs

| Field              | Type        | Description                            |
|-------------------|-------------|----------------------------------------|
| metric_id         | UUID        | Primary Key                            |
| model_id          | UUID        | Foreign key to Models                  |
| name              | String      | Metric name (e.g., EBITDA Margin)      |
| value             | Float       | Calculated value                       |
| period            | Enum        | Monthly, Quarterly, Annual             |
| timestamp         | Timestamp   | When metric was last updated           |

---

## 📁 Files

| Field              | Type        | Description                            |
|-------------------|-------------|----------------------------------------|
| file_id           | UUID        | Primary Key                            |
| model_id          | UUID        | Related model                          |
| user_id           | UUID        | Uploaded by                            |
| file_name         | String      | Original file name                     |
| file_type         | Enum        | Excel, CSV, Google Sheet               |
| storage_url       | String      | S3 or storage link                     |
| created_at        | Timestamp   | Upload time                            |

---

## 📈 Insights

| Field              | Type        | Description                            |
|-------------------|-------------|----------------------------------------|
| insight_id        | UUID        | Primary Key                            |
| model_id          | UUID        | Related model                          |
| category          | Enum        | VC, PE, General                        |
| content           | Text        | Insight/commentary generated           |
| generated_by      | Enum        | AI, User, Analyst                      |
| created_at        | Timestamp   | Time of generation                     |

---

## 🔄 Model Versions

| Field              | Type        | Description                            |
|-------------------|-------------|----------------------------------------|
| version_id        | UUID        | Primary Key                            |
| model_id          | UUID        | Related model                          |
| version_number    | Integer     | Auto-incremented                       |
| assumptions_json  | JSONB       | Snapshot of assumptions                |
| metrics_json      | JSONB       | Snapshot of KPIs                       |
| created_by        | UUID        | User who saved the version             |
| created_at        | Timestamp   | Time of version creation               |

---

## 🔐 Permissions

| Field              | Type        | Description                            |
|-------------------|-------------|----------------------------------------|
| permission_id     | UUID        | Primary Key                            |
| user_id           | UUID        | Foreign key to Users                   |
| model_id          | UUID        | Foreign key to Models                  |
| access_level      | Enum        | View, Comment, Edit                    |
| granted_by        | UUID        | Who granted access                     |
| granted_at        | Timestamp   | When access was granted                |

---

## 📌 Activity Logs (Optional)

| Field              | Type        | Description                            |
|-------------------|-------------|----------------------------------------|
| log_id            | UUID        | Primary Key                            |
| user_id           | UUID        | Related user                           |
| model_id          | UUID        | Related model                          |
| action_type       | String      | e.g., Updated Assumption, Generated Model |
| details           | JSONB       | Metadata about the action              |
| timestamp         | Timestamp   | When the action occurred               |

---

## Entity Relationships (Simplified)

```plaintext
Users --< Models
Users --< Assumptions
Models --< Assumptions
Models --< Metrics
Models --< Insights
Models --< Files
Models --< ModelVersions
Users --< Permissions
Organizations --< Users
Organizations --< Models
