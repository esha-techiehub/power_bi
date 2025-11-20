# 📘 Cardinality in Power BI — Detailed Beginner-Friendly Notes

## ⭐ What Is Cardinality?
Cardinality describes **how many rows in one table match how many rows in another table** in Power BI.  
It defines the *type of relationship* between two tables.

---

# 🟩 1. One-to-Many (1:*) — MOST IMPORTANT

## ✔ Meaning
One row in Table A matches **multiple** rows in Table B.

## 📦 Example: Customer → Sales
```
Customers (1)
     |
     ↓
Sales (*)
```

### Customer Table
| CustomerID | Name |
|------------|------|
| C101       | John |

### Sales Table
| SaleID | CustomerID | Amount |
|--------|------------|--------|
| 1      | C101       | 200    |
| 2      | C101       | 300    |

### ✔ Why this is correct
- One customer can place many sales.
- Filters work correctly.
- Most star schema models use this.

---

# 🟦 2. Many-to-One (*:1)

## ✔ Meaning
Same as one-to-many, but reversed direction in Power BI interface.

```
Sales (*) ——→ Customers (1)
```

Used commonly when Power BI auto-detects the relationship backwards.

---

# 🟨 3. Many-to-Many (*:*) — Use Carefully

## ✔ Meaning
Rows in Table A match multiple rows in Table B  
AND  
Rows in Table B match multiple rows in Table A.

## 📘 Real Example: Student ↔ Course
```
Students (*) ←→ (*) Courses
```

### ⚠ Issues with Many-to-Many
- Duplicate values  
- Wrong totals  
- Confusing filter behavior  
- Breaks RLS  

### ⭐ Best Practice
Use a **bridge table**.

---

# 🟥 4. One-to-One (1:1)

## ✔ Meaning
One row in Table A matches exactly one row in Table B.

## 📘 Example
EmployeeDetails (1) —— EmployeeSalary (1)

Rare in BI models.

---

# 🎯 Why Cardinality Matters

Cardinality controls:
- How filters flow between tables  
- How visuals aggregate values  
- Whether totals are correct  
- Whether slicers work properly  
- Model performance  

## ❌ Wrong Cardinality = Wrong Results

Example: Product table joined incorrectly to Sales as many-to-many.

Results:
- Duplicate totals  
- Wrong filters  
- Blank rows  

---

# ⭐ Choosing Correct Cardinality

| Scenario | Correct Cardinality |
|----------|----------------------|
| Customer → Sales | 1:* |
| Product → Sales | 1:* |
| Date → Sales | 1:* |
| Region → Customers | 1:* |
| Student ↔ Course | *:* (with bridge) |
| Employee ↔ Salary | 1:1 |

---

# 🎓 Easy Analogy

### ✔ One-to-Many
One teacher → many students

### ✔ Many-to-Many
Many students → many activities

### ✔ One-to-One
One student → one ID card

---

# 🎉 End of Notes
