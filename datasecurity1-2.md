# 📘 Power BI Security 

---

# ============================================================
# ✅ SECTION 1 — INTRODUCTION TO POWER BI SECURITY
# ============================================================

---

# 1.1 What is Data Security in Power BI?

## 📌 Definition
Data security in Power BI includes techniques and controls used to ensure:
- Only authorized users access specific data
- Sensitive data is protected
- Users see only the portion of data they’re permitted to see (principle of least privilege)

## 🔍 Why It Matters
Power BI is a **self-service BI tool**, so many users can:
- Load data
- Build reports
- Share dashboards

Without security:
- Confidential data may leak
- Employees can see private data (salary, sales commissions, etc.)
- Mistakes may cause organization-wide exposure
- Compliance laws (GDPR, HIPAA) can be violated

## 📊 Simple Diagram
```
              +---------------------------+
              |     Power BI Security     |
              +---------------------------+
                /         |                    Data Access   Data View   Data Sharing
```

## 💡 Example
A company's data model contains:
- Sales table
- Region table
- Customer details

Security needs:
- Sales Manager → access all regions
- Salesperson → only their own region
- Finance → revenue but not customer details
- HR → separate data, hidden from others

## 🎓  Tip
Ask students:
> “What information in your company should only be seen by certain departments?”

## 📝 Class Exercise
List **three sensitive data fields** that must be restricted from general employees.

---

# 1.2 Why Security Matters in Self-Service BI

## 📌 Definition
Self-service BI means employees can load, explore, and publish content on their own.

This increases productivity **but also risk** if not controlled.

## ⚠️ Risks If Security Is Ignored
- Salary or HR data might leak
- Users may see confidential financial performance
- Someone might publish sensitive dashboards to the whole company
- Legal & compliance issues (GDPR, HIPAA, ISO)

## 📊 Diagram
```
         Self-Service BI (Everyone Builds Reports)
                       |
               Without Security
                       |
        +----------------------------------+
        | Data leakage, wrong decisions,    |
        | compliance issues, reputation     |
        | damage                            |
        +----------------------------------+
```

## 💡 Example
An HR intern imports a table containing employee salaries → publishes a report → accidentally shares with the entire company.

## 🎓  Tip
Explain:
> “Self-service BI becomes dangerous without strong governance.”

## 📝 Class Exercise
Ask students to share **one example** of how wrong access could damage an organization.

---

# 1.3 Security Across the Power BI Ecosystem

## 📌 Definition
Power BI security spans across three areas:
- **Power BI Desktop** → Build the security model  
- **Power BI Service** → Enforce user access and sharing  
- **Data Gateway** → Secure on-premise connections  

## 📊 Diagram
```
+-------------------+     +------------------+     +----------------+
| Power BI Desktop  | --> | Power BI Service | --> | Data Gateway   |
| (Build Security)  |     | (Enforce Access) |     | (Secure Source)|
+-------------------+     +------------------+     +----------------+
```

## 💡 Example Workflow
1. Create RLS roles in Desktop  
2. Publish to Service  
3. Assign users to RLS roles  
4. Gateway refreshes SQL Server securely  

## 🎓  Tip
Tell students:
> “Security is *defined* in Desktop but *activated* in the Service.”

## 📝 Class Exercise
Draw the flow diagram and write **one security feature** under each component.

---

# ============================================================
# ✅ SECTION 2 — LAYERS OF SECURITY IN POWER BI
# ============================================================

Power BI security operates across multiple layers:
- Data source  
- Dataset  
- Workspace  
- Sharing  

---

# 2.1 Data Source Security

## 📌 Definition
Data source security refers to authentication and credential management for connecting Power BI to external data systems.

---

## 🔐 Database-Level Authentication

### 📌 Meaning
Power BI authenticates into data sources like:
- SQL Server
- Oracle
- SAP
- Azure SQL
- Web APIs

Types of authentication:
- Windows AD login
- SQL username/password
- OAuth tokens
- Organization account

### 📊 Diagram
```
Power BI → Authentication → Database → Authorized Data Returned
```

### 💡 Example
A Power BI user connects to SQL using AD credentials.  
SQL Server allows them to view:
- Sales  
- Customers  
But denies access to HRSalary table → protected at database level.

### 🎓  Tip
Explain:
> “Power BI security starts at the database itself.”

### 📝 Class Exercise
List **3 data sources** requiring authentication.

---

## 🔑 Credentials & Connection Security

### 📌 Meaning
Credentials used for refreshing datasets must be stored in the Power BI Service.

### Problems If Not Configured:
- Dataset refresh fails
- Gateway cannot authenticate
- User-level credentials may not exist in the cloud

### 💡 Example
You connect with your Windows account in Desktop, but the Service has no gateway with your credentials → refresh fails.

### 🎓  Tip
Explain the difference between:
- Desktop credentials  
- Service credentials  

### 📝 Class Exercise
Why does Power BI require credentials in both Desktop and the Service?

---

# 2.2 Dataset Security

## 📌 Definition
Dataset security determines who can view, build, or modify reports based on a published dataset.

---

## 🔐 Permissions on Datasets

### Types of Permissions
| Permission | What It Allows |
|-----------|------------------|
| **Read** | Open reports |
| **Build** | Create new reports on top of dataset |
| **Reshare** | Share dataset with others |
| **Download** | Export PBIX (high risk) |

### 💡 Example
Give users Read permission only if you don't want them creating personal copies of reports.

---

## 👥 Ownership & Workspace Roles

### Workspace Roles
| Role | Capabilities |
|------|--------------|
| Admin | Full control |
| Member | Edit + share |
| Contributor | Edit only |
| Viewer | View only |

### 💡 Example
Finance Workspace:
- CFO → Admin  
- Analysts → Member  
- Executives → Viewer  

### 📊 Diagram
```
Workspace
 ├─ Admin (Full)
 ├─ Member (Edit+Share)
 ├─ Contributor (Edit)
 └─ Viewer (Read)
```

---

# 2.3 Workspace Security

## 📌 Definition
Controls permissions within a workspace for:
- Reports  
- Dashboards  
- Datasets  
- Dataflows  

### 📊 Diagram
```
Workspace
 ├── Datasets
 ├── Reports
 ├── Dashboards
 └── Dataflows
```

### 💡 Example
A Viewer cannot:
- Edit reports  
- Create datasets  
- Publish new versions  

They can only consume content.

---

# 2.4 Sharing & App Security

## 🔗 Share Report vs Publish to App

### Share Report
- One-off sharing
- Only specific item
- Not scalable

### Publish to App
- Bundle multiple reports/dashboards
- Create audiences
- Best for enterprise distribution

### 💡 Example
The Sales App includes:
- Sales Summary Dashboard  
- Region Performance  
- Daily KPIs  

---

## 🌍 Organizational vs External Access

### Organizational Access
Only company users can see reports.

### External Access
Vendors / partners can see content if enabled by admin.

---

## 🏷️ Sensitivity Labels (Purview)

### 📌 Meaning
Classification applied to Power BI data:
- Public  
- Confidential  
- Highly Confidential  
- Restricted  

Controls:
- Download  
- Export  
- Copy/paste  
- Screenshot prevention (depending on policy)

### 💡 Example
Customer contact data is marked **Highly Confidential**, preventing exports.

### 🎓  Tip
Tell students:
> “Sensitivity labels travel with the data—even into Excel.”

---

# END OF SECTIONS 1 & 2
# ============================================================
