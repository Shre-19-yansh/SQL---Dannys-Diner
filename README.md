# 🍜 Case Study #1: Danny's Diner
Analysis of Danny’s Diner customer data using SQL. Explore sales, menu, and membership datasets to uncover customer patterns, spending habits, and favorite menu items to support business decisions and loyalty program expansion.
<img width="836" height="859" alt="Screenshot 2025-08-22 120411" src="https://github.com/user-attachments/assets/c4a5e8d2-9873-4690-89c0-0ea4960ba061" />

---

## 📂 Files in This Repository
- **Data Files**
  - `Member.csv` → Customer membership information  
  - `Menu.csv` → Menu details  
  - `Sales.csv` → Customer orders  

- **Scripts**
  - `Case_Study_Answer.md` → Contains all question, SQL queries and answers to the case study questions  
  - `README.md` → Information about the project, process, and objectives  

---

## 📝 Project Overview
Danny’s Diner is a small Japanese restaurant looking to understand its customers better. This project analyzes sales, menu, and membership data using SQL to uncover visiting patterns, spending habits, and favorite dishes. Insights aim to guide business decisions and loyalty program expansion.

---

## 🎯 Objectives
- Analyse customer visiting patterns to understand frequency and behaviour  
- Calculate total money spent by customers to identify high-value customers  
- Discover most popular menu items to see customer preferences  

---

## 📌 Expected Outcomes
These insights will help Danny:  
- Personalize the dining experience for loyal customers  
- Evaluate whether to expand the customer loyalty program  
- Generate simple, ready-to-use datasets that his team can inspect without SQL knowledge  

Danny has provided sample datasets containing customer orders, menu details, and membership information.  
Using SQL queries (see **SQL Script.sql**), we aim to clean, prepare, and analyse this data to answer his key business questions.  

---

## 🛠️ Tools & Setup
- **Database:** MySQL  
- **Steps:**
  1. Import the required datasets (`Members.csv`, `Menu.csv`, `Sales.csv`)  
  2. Set up **Danny’s Diner Schema**  

---

## 📊 Process

### 1️⃣ Data Collection
- Use the provided datasets:  
  - `Members.csv`  
  - `Sales.csv`  
  - `Menu.csv`  

### 2️⃣ Database Setup
- Created **Danny’s Diner Schema**  
- Imported the datasets into MySQL  

### 3️⃣ Initial Data Exploration
- Performed a basic overview of the tables  
- Checked for missing values, duplicates, and consistency  

### 4️⃣ Relational Database Development
- Defined relationships between the three tables:  
  - **Sales** → customer orders  
  - **Menu** → menu details  
  - **Members** → customer membership information  
- Established **primary and foreign keys** to ensure proper joins
- <img width="1069" height="518" alt="Screenshot 2025-08-22 143220" src="https://github.com/user-attachments/assets/4cd5f51a-4d23-4267-b7dd-89fbb7c4be93" />
  

### 5️⃣ SQL Query Development
- Wrote queries (see **SQL Script.sql**) to answer key business questions:  
  - Customer visiting patterns  
  - Total money spent by each customer  
  - Most popular menu items  

### 6️⃣ Insights & Reporting
- Summarized findings to support Danny’s decision-making  
- Generated datasets that can be used by the team without SQL  

---
