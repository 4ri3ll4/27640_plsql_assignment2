
# PL/SQL Practical Assignment Report  
### Tasks 1 – 3: Oracle Database and OEM Configuration  
**Prepared by:** Ariella  
**Student ID:** 27640  
**Course:** PL/SQL  

---

## 🧩 Task 1: Create a New Pluggable Database (PDB)

### **Objective**
Create a new Pluggable Database that will store all class work.

### **Steps Performed**
1. Connected to Oracle SQL Developer using **SYS as SYSDBA**.  
2. Verified container environment:
   ```sql
   SHOW CON_NAME;
   SHOW PDBS;
   ```
3. Created a new PDB named **ar_pdb_27640**:
   ```sql
   CREATE PLUGGABLE DATABASE ar_pdb_27640
   ADMIN USER ariella_plsqlauca_27640 IDENTIFIED BY StrongPass123!
   FILE_NAME_CONVERT = ('pdbseed', 'ar_pdb_27640');
   ```
4. Opened the PDB and saved its state:
   ```sql
   ALTER PLUGGABLE DATABASE ar_pdb_27640 OPEN;
   ALTER PLUGGABLE DATABASE ar_pdb_27640 SAVE STATE;
   ```
5. Verified it was active:
   ```sql
   SHOW PDBS;
   ```

### **Notes**
- The **ADMIN USER** `ariella_plsqlauca_27640` will be used for all PL/SQL exercises.  
- Passwords in Oracle are case-sensitive.  
- The connection for this PDB was created in SQL Developer using:
  - **Username:** ariella_plsqlauca_27640  
  - **Password:** StrongPass123!  
  - **Service Name:** ar_pdb_27640  
  - **Role:** Default  

### **Observation**
Initial connection attempts failed due to `ORA-01017: invalid username/password; logon denied`.  
This was resolved by ensuring the connection was directed to the **PDB service name** (`ar_pdb_27640`) instead of the default `XEPDB1`.

---

## 🧩 Task 2: Create and Delete a PDB

### **Objective**
Create a secondary PDB and delete it after verifying successful creation.

### **Steps Performed**
1. Connected again as **SYS AS SYSDBA**.
2. Created another PDB:
   ```sql
   CREATE PLUGGABLE DATABASE ar_to_delete_pdb_27640
   ADMIN USER ariella_delete_27640 IDENTIFIED BY DeleteMe123!
   FILE_NAME_CONVERT = ('pdbseed', 'ar_to_delete_pdb_27640');
   ```
3. Verified the new PDB was created:
   ```sql
   SHOW PDBS;
   ```
4. Opened it to test accessibility:
   ```sql
   ALTER PLUGGABLE DATABASE ar_to_delete_pdb_27640 OPEN;
   ```
5. Then closed and dropped it:
   ```sql
   ALTER PLUGGABLE DATABASE ar_to_delete_pdb_27640 CLOSE IMMEDIATE;
   DROP PLUGGABLE DATABASE ar_to_delete_pdb_27640 INCLUDING DATAFILES;
   ```

### **Notes**
- Confirmed successful deletion by running `SHOW PDBS;` again.  
- **Screenshots** of both creation and deletion were taken for documentation.  

### **Observation**
Oracle required explicit inclusion of `INCLUDING DATAFILES` to ensure complete removal of the PDB and avoid leftover file conflicts.

---

## 🧩 Task 3: Oracle Enterprise Manager (OEM)

### **Objective**
Configure and access **Oracle Enterprise Manager Express (EM Express)** to verify the database setup.

### **Steps Performed**
1. Verified database and listener were running:
   ```bash
   lsnrctl status
   sqlplus / as sysdba
   STARTUP;
   ```
2. Checked and configured EM Express port:
   ```sql
   SELECT dbms_xdb_config.getHttpsPort() FROM dual;
   EXEC DBMS_XDB_CONFIG.SETHTTPSPORT(5500);
   ```
3. Opened the URL:
   ```
   https://localhost:5500/em
   ```
4. Logged in using:
   - **Username:** ariella_plsqlauca_27640  
   - **Password:** StrongPass123!  
   - **Service Name:** ar_pdb_27640  
5. Captured the dashboard showing:
   - Database: ar_pdb_27640  
   - Username: ariella_plsqlauca_27640  

### **Notes**
- The connection may display as *“Not Secure”* due to a self-signed SSL certificate.  
  This is normal for local development and does not indicate a real security risk.
- A screenshot of the OEM dashboard was captured for submission.

### **Observation**
If EM Express fails to load, ensure both the **Oracle Listener** and **Database services** are running.  
Port 5500 must remain open for EM Express to function.

---

## 📸 Screenshots Section
- Screenshot 1: Creation of `ar_pdb_27640`
- Screenshot 2: Creation and deletion of `ar_to_delete_pdb_27640`
- Screenshot 3: Oracle EM Express Dashboard (showing username and database)

---

## 🧠 Issues and Solutions

| Issue | Cause | Solution |
|-------|--------|-----------|
| ORA-01017 invalid username/password | Wrong service name in SQL Developer | Updated connection to use `ar_pdb_27640` |
| EM Express “Page not secure” warning | Self-signed SSL certificate | Proceeded safely (local connection) |
| PDB deletion error | Missing `INCLUDING DATAFILES` clause | Added `INCLUDING DATAFILES` to drop command |

---

## 🧾 Summary
This practical involved creating and managing Pluggable Databases (PDBs) in Oracle, as well as configuring Oracle Enterprise Manager Express.  
Through this exercise, I learned how to:  
- Create, open, and drop PDBs.  
- Manage users and resolve connection issues.  
- Configure and access EM Express for database monitoring.  

All objectives were successfully met. ✅

---
