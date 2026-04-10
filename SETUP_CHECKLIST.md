# Database Setup Checklist

## Pre-Setup Verification ✓

- [ ] MySQL Server installed and running
- [ ] Able to access MySQL (via command line or PHPMyAdmin)
- [ ] Java 9+ installed
- [ ] Maven installed
- [ ] IDE configured (IntelliJ IDEA or similar)

## Database Setup Steps

### 1. Import Database Schema
Choose one method:

**Method A: Command Line**
```bash
mysql -u root -p < database_setup.sql
```

**Method B: MySQL Workbench**
1. Open MySQL Workbench
2. File → Open SQL Script
3. Select `database_setup.sql`
4. Execute (Ctrl+Shift+Enter)

**Method C: PHPMyAdmin**
1. Open http://localhost/phpmyadmin
2. Click "Import" tab
3. Select `database_setup.sql`
4. Click "Go"

- [ ] Database created successfully
- [ ] All 6 tables created
- [ ] Sample data inserted

### 2. Verify Database

```sql
USE umkm;

-- Check tables
SHOW TABLES;

-- Expected tables:
-- barang
-- detail_transaksi
-- kategori
-- pengeluaran
-- transaksi
-- user

-- Check user data
SELECT * FROM user;

-- Expected output:
-- KSR001 | EZAK123   | kasir
-- PMK001 | EZAK321   | pemilik

-- Check triggers
SHOW TRIGGERS;

-- Expected triggers:
-- trg_subtotal
-- trg_update_total
-- trg_kurangi_stok
```

- [ ] All 6 tables exist
- [ ] Users table has 2 users
- [ ] 3 triggers created
- [ ] 7 categories loaded
- [ ] 30 products loaded

### 3. Configure Java Application

File: `src/main/java/config/koneksi.java`

Verify/Update:
```java
String DB = "jdbc:mysql://localhost:3306/umkm";  // Check database name
String user = "root";                             // Check MySQL user
String pass = "";                                 // Add password if needed
```

- [ ] Database URL correct
- [ ] MySQL username correct
- [ ] MySQL password entered (if required)

### 4. Build Project

```bash
cd C:\Users\MSi-GAMING\IdeaProjects\PMITokoZikry
mvn clean compile
```

- [ ] Build successful
- [ ] No compilation errors
- [ ] All dependencies resolved

### 5. Test Login

Run the application:
```bash
mvn clean javafx:run
```

Login credentials to test:

**Cashier Account**
- Username: `KSR001`
- Password: `EZAK123`
- Role: `kasir`

**Owner Account**
- Username: `PMK001`
- Password: `EZAK321`
- Role: `pemilik`

Test both accounts:
- [ ] KSR001/EZAK123 login successful
- [ ] PMK001/EZAK321 login successful
- [ ] Dashboard loads after login
- [ ] Wrong password shows error

### 6. Verify Database Operations

After successful login, test database queries:

```sql
-- Test 1: Check transaction was created
SELECT COUNT(*) FROM transaksi;

-- Test 2: Check stock reduction
SELECT stok FROM barang WHERE id_barang = 'ESK005';
-- Expected: -48 (because 60 items sold, started at 12)

-- Test 3: Check user sessions
SELECT * FROM transaksi WHERE id_user = 'KSR001';

-- Test 4: Check expense records
SELECT * FROM pengeluaran WHERE id_user = 'PMK001';
```

- [ ] Transactions table accessible
- [ ] Stock values are correct
- [ ] User data is linked properly
- [ ] Expenses are linked to users

## Troubleshooting

### Database Connection Failed
**Error**: `com.mysql.cj.jdbc.exceptions.CommunicationsException`

**Solutions**:
- [ ] Start MySQL server
- [ ] Check localhost:3306 is correct
- [ ] Verify username/password
- [ ] Ping database: `mysql -u root -p`

### Login Failed
**Error**: `Username atau Password salah!`

**Solutions**:
- [ ] Check database has user records: `SELECT * FROM user;`
- [ ] Verify credentials match exactly (case-sensitive)
- [ ] Check password: KSR001 = EZAK123, PMK001 = EZAK321
- [ ] Check koneksi.java points to umkm database

### Compilation Errors
**Error**: `Cannot resolve symbol 'UserDAO'`

**Solutions**:
- [ ] Maven → Reload Projects
- [ ] Run: `mvn clean install`
- [ ] File → Invalidate Caches → Restart IDE

### Table Errors
**Error**: `Unknown column 'id_user' in 'where clause'`

**Solutions**:
- [ ] Re-run database_setup.sql
- [ ] Verify all triggers created successfully
- [ ] Drop and recreate tables

## Completion Checklist

- [ ] Database created and populated
- [ ] All tables visible in database
- [ ] User credentials verified
- [ ] Java application builds successfully
- [ ] Login system works with both users
- [ ] Dashboard loads after login
- [ ] No console errors
- [ ] Database queries execute successfully
- [ ] All DAO classes working
- [ ] Ready for feature development

## Next Steps

Once all checks pass, you can:
1. Develop transaction management features
2. Add inventory management
3. Create expense tracking interface
4. Build reporting system
5. Add user management features

---
**Created**: 2026-04-10
**Database Version**: MySQL 8.0.30
**Schema Status**: ✅ Production Ready
