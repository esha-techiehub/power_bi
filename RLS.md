# 📘 Section 3 — Power BI Row-Level Security (RLS)

Row-Level Security (RLS) restricts data at the **row level**, ensuring each user sees only the data they are authorized to view.

---

# ============================================================
# 3.1 What is RLS and Why It’s Used
# ============================================================

## 📌 Definition
RLS filters data in a model so different users see different subsets of data.

## 🎯 Purpose
- Protect sensitive information  
- Ensure correct data visibility  
- Enforce compliance  
- Prevent accidental data exposure  

## 💡 Example
Salesperson from **Region = North** sees only North data.  
Sales Manager sees all regions.

---

# ============================================================
# 3.2 How RLS is Applied (Desktop vs Service)
# ============================================================

## 🖥️ Power BI Desktop
- Create RLS roles  
- Write DAX filters  
- Test using “View As Role”  

## 🌐 Power BI Service
- Assign users to roles  
- Enforce security during report access  

### 📊 Diagram
```
Desktop (Create Role) →
Publish →
Service (Assign Users) →
Report (RLS Active)
```

---

# ============================================================
# 3.3 Static RLS
# ============================================================

## 📌 Definition
Filters are fixed (hardcoded).

## 💡 Example
Role: **Region_North**  
Filter: `Region = "North"`

## 📝 Use Case
Small organizations with simple region-based access.

---

# ============================================================
# 3.4 Dynamic RLS
# ============================================================

## 📌 Definition
DAX uses the logged-in user’s email to filter rows dynamically.

## Example Formula
```DAX
Region[SalespersonEmail] = USERPRINCIPALNAME()
```

## 📝 Use Case
Many users with individualized access:
- Salesperson → own customers  
- Agents → own accounts  

---

# ============================================================
# 3.5 Creating Roles & DAX Filters
# ============================================================

### Steps in Desktop
1. Modeling → Manage Roles  
2. Create role (e.g., Sales_Role)  
3. Add DAX filter  
4. Save  

### Example Filters
- Region-level: `Region = "South"`  
- Manager-level: `Region IN VALUES(UserRegion[Region])`  
- Dynamic user security:  
```DAX
Users[Email] = USERPRINCIPALNAME()
```

---

# ============================================================
# 3.6 Testing RLS in Power BI Desktop
# ============================================================

### Steps:
Modeling → View As → Select Role → Check Data

### 💡 Example
Test “North Region” role and ensure only North rows appear.

---

# ============================================================
# 3.7 Assigning Users to Roles in Power BI Service
# ============================================================

### Steps:
1. Publish dataset  
2. Go to Dataset → Security  
3. Select Role  
4. Add Users / Security Groups  

### 🎯 Best Practice
Use **Azure AD Security Groups** instead of individual users.

---

# ============================================================
# 3.8 Common Mistakes & Best Practices
# ============================================================

## ❌ Common Mistakes
- RLS defined but users not assigned  
- Using individual emails instead of groups  
- Incorrect model relationships  
- Ambiguous relationships causing RLS leaks  

## ✅ Best Practices
- Use GROUPS (AAD)  
- Test every role  
- Keep model clean with correct relationships  
- Document security rules  

---

# 🎉 End of Section 3



# 🧪 Power BI Row-Level Security (RLS) — Full Step-by-Step Lab for Beginners (Layman Friendly)

This lab is written **very simply**, assuming you are a beginner.  
Every step includes exact clicks, screenshots guidance, and expected results.

---

# ✅ What You Will Learn
By the end of this lab, you will be able to:

- Load data into Power BI  
- Build a simple data model  
- Create **Static RLS**  
- Create **Dynamic RLS**  
- Test roles  
- Publish to Power BI Service  
- Assign users to roles and verify security  

---

# 📂 Files You Will Need (Create These Files)

Create 3 `.csv` files on your computer:

### 1️⃣ Sales.csv
```csv
SalesID,Date,Region,SalespersonEmail,Customer,Amount
1,2025-01-01,North,john.north@company.com,Alpha,1200
2,2025-01-03,South,sara.south@company.com,Beta,900
3,2025-01-05,East,eric.east@company.com,Gamma,1500
4,2025-01-06,West,wendy.west@company.com,Delta,700
5,2025-01-08,North,john.north@company.com,Omega,3000
```

### 2️⃣ Regions.csv
```csv
Region,RegionManagerEmail
North,nina.northmgr@company.com
South,sam.southmgr@company.com
East,eva.eastmgr@company.com
West,will.westmgr@company.com
```

### 3️⃣ Users.csv
```csv
UserEmail,RoleType,Region
john.north@company.com,SalesRep,North
sara.south@company.com,SalesRep,South
eric.east@company.com,SalesRep,East
wendy.west@company.com,SalesRep,West
nina.northmgr@company.com,RegionManager,North
sam.southmgr@company.com,RegionManager,South
eva.eastmgr@company.com,RegionManager,East
will.westmgr@company.com,RegionManager,West
ceo@company.com,HeadOffice,ALL
```

---

# 🧪 LAB 1 — Load the Data into Power BI Desktop

## Step 1 — Open Power BI Desktop  
Just double-click **Power BI Desktop** on your computer.

---

## Step 2 — Import Sales.csv  
1. Click **Home → Get Data → Text/CSV**  
2. Select **Sales.csv**  
3. Click **Load**

You should now see a table called **Sales** on the right side (Fields Pane).

---

## Step 3 — Import Regions.csv  
Repeat the same steps:
1. Home → Get Data → Text/CSV  
2. Choose **Regions.csv**  
3. Click **Load**

---

## Step 4 — Import Users.csv  
Again:
1. Home → Get Data → Text/CSV  
2. Select **Users.csv**  
3. Load  

---

# 🧪 LAB 2 — Build the Data Model

## Step 1 — Go to Model View
On the left side, click the icon that looks like 3 squares (Model View).

You will see all 3 tables.

---

## Step 2 — Create Relationship  
Drag `Regions[Region]` → `Sales[Region]`.

A line will appear.

✔ Expected result:  
**1 Region has many Sales rows.**

You do NOT need to join Users table.

---

# 🧪 LAB 3 — Create Static RLS (Beginner-Friendly)

Static RLS means you manually filter data for a role.

## Step 1 — Open RLS window
Click:

**Modeling → Manage Roles**

---

## Step 2 — Create “North Only” Role
1. Click **Create**  
2. Name the role: **RLS_North**  
3. Select **Sales** table  
4. In the filter box type:

```DAX
[Region] = "North"
```

✔ Expected result:  
This role will show only rows where Region = North.

---

## Step 3 — Repeat for other regions  
Create:
- `RLS_South` → `[Region] = "South"`
- `RLS_East` → `[Region] = "East"`
- `RLS_West` → `[Region] = "West"`

---

# 🧪 LAB 4 — Test Static RLS

## Step 1 — Click “View As”
Go to:

**Modeling → View As**

## Step 2 — Select RLS_North  
Click **OK**

✔ Expected result:  
Only **North** rows appear in all visuals.

Try other roles too.

---

# 🧪 LAB 5 — Create Dynamic RLS

Dynamic RLS uses login email to filter data automatically.

---

## Step 1 — Open Manage Roles  
Click:

**Modeling → Manage Roles → Create**

Name the role: `RLS_DynamicUsers`

---

## Step 2 — Select Sales Table  
Click **Sales** table.

---

## Step 3 — Enter Dynamic DAX

Copy and paste:

```DAX
VAR CurrentRegion =
    LOOKUPVALUE(
        Users[Region],
        Users[UserEmail], USERPRINCIPALNAME()
    )
RETURN
IF(
    CurrentRegion = "ALL",
    TRUE(),
    Sales[Region] = CurrentRegion
)
```

✔ Explanation for beginners:
- This finds your email in Users table  
- Gets your region  
- Filters sales to that region  
- If region = ALL → shows everything  

---

# 🧪 LAB 6 — Test Dynamic RLS

Go to:

**Modeling → View As → Other User**

Test with:

- `john.north@company.com` → Only North  
- `sara.south@company.com` → Only South  
- `ceo@company.com` → All data  

✔ Expected results shown in your visuals.

---

# 🧪 LAB 7 — Publish to Power BI Service

## Step 1 — Click Publish
Home → **Publish**

Choose a workspace (My Workspace is fine for practice).

---

# 🧪 LAB 8 — Assign Users to RLS Roles in Service

1. Go to www.powerbi.com  
2. Open your workspace  
3. Click **Datasets + Dataflows**  
4. Find your dataset  
5. Click **… → Security**  

You will now see:
- RLS_North  
- RLS_South  
- RLS_East  
- RLS_West  
- RLS_DynamicUsers  

---

## Step 1 — Assign users  
Click on a role → add user email.

Example:
- Add `john.north@company.com` → RLS_North  
- Add all users → RLS_DynamicUsers  

---

# 🧪 LAB 9 — Test RLS in Power BI Service

1. Open your dataset → Security  
2. Click **Test as Role**  
3. Choose any role  
4. View report  

✔ Expected result:  
Only that role’s data is visible.

---

# 🎉 LAB COMPLETED — You Now Understand RLS End-to-End!

---

# 🧠 Concepts Learned

- Loading CSV data  
- Creating relationships  
- Creating Static RLS  
- Creating Dynamic RLS  
- Using USERPRINCIPALNAME()  
- Testing RLS  
- Publishing  
- Assigning users  
- Testing in Service  

---

# 🏆 Bonus Challenges (Optional)

Try these once you are comfortable:

### 1. Multi-region sales reps  
Change Users table to assign multiple regions.

### 2. Add OLS  
Hide columns like:
- Cost  
- Margin  
- Internal Notes  

### 3. Add Hierarchy RLS  
Using `PATH()` and `PATHCONTAINS()`  
create manager → employee tree.

---

If you need a **video demo**, **PowerPoint version**, or **combined master file**, just tell me!

