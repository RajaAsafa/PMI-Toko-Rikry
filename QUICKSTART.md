# 🚀 QUICK START - 5 Minutes to Running

## ⚡ Super Quick Setup

### Step 1: Import Database (2 minutes)
Copy this command:
```bash
mysql -u root -p < database_setup.sql
```
Then press Enter and type your MySQL password (or just Enter if no password).

**Verify it worked:**
```sql
mysql -u root -p
> USE umkm;
> SELECT * FROM user;
```
You should see:
```
KSR001 | EZAK123 | kasir
PMK001 | EZAK321 | pemilik
```

### Step 2: Build Project (2 minutes)
```bash
cd C:\Users\MSi-GAMING\IdeaProjects\PMITokoZikry
mvn clean compile
```

### Step 3: Run Application (1 minute)
```bash
mvn clean javafx:run
```

### Step 4: Login
**Use either:**
- Username: `KSR001` | Password: `EZAK123` (Cashier)
- Username: `PMK001` | Password: `EZAK321` (Owner)

✅ **Done!** You're in the system.

---

## 🎯 What You Get

✅ Complete database with 6 tables
✅ User authentication (KSR001, PMK001)
✅ 30 products across 7 categories
✅ Automatic calculations (subtotal, total, stock)
✅ Sample transaction & expenses
✅ Login system integrated

---

## 📂 File Guide

| File | Purpose |
|------|---------|
| `database_setup.sql` | **Import this first!** Contains all tables + data |
| `DATABASE_SETUP.md` | Database configuration details |
| `DATABASE_STRUCTURE.md` | Table relationships & schema |
| `SETUP_CHECKLIST.md` | Step-by-step verification |
| `INTEGRATION_SUMMARY.md` | What was integrated |
| `INTEGRATION_COMPLETE.md` | Overview & status |
| `README_INTEGRATION.md` | Full technical reference |

---

## ⚠️ Common Issues & Fixes

### "Can't connect to MySQL"
```bash
# Make sure MySQL is running
# Windows: Start menu → MySQL
# Then try: mysql -u root -p
```

### "Access denied for user 'root'"
```bash
# Edit: src/main/java/config/koneksi.java
# Change: String pass = ""; 
# To: String pass = "your_password";
```

### "Build failed"
```bash
# Clean and rebuild
mvn clean install
```

### "Can't find database"
```sql
-- Check database was created
mysql -u root -p
> SHOW DATABASES;
-- Should show 'umkm'
```

---

## 📊 What's in the Database?

- 🔐 **Users**: 2 accounts (KSR001, PMK001)
- 📦 **Products**: 30 items (ice cream, drinks, food, etc)
- 🏷️ **Categories**: 7 (Es Krim, Minuman, Makanan, Pembersih, Rokok, Sembako, Lain-lain)
- 🛒 **Transactions**: Sample transaction with 60 items sold
- 💰 **Expenses**: Sample expenses (PLN, Water, Packaging)
- ⚙️ **Triggers**: 3 automatic calculations

---

## 🔑 Login Test

After running the app:

**Test 1: Cashier Login**
- User: `KSR001`
- Pass: `EZAK123`
- Expected: Access to sales features

**Test 2: Owner Login**
- User: `PMK001`
- Pass: `EZAK321`
- Expected: Access to reports & settings

---

## 📈 Next Steps

After verifying login works:

1. **Add transaction form** - Let cashier enter new sales
2. **Add product list** - Show inventory with stock levels
3. **Add expense form** - Let owner record expenses
4. **Add reports** - Daily sales, expenses, stock

---

## 💡 Useful Commands

```bash
# Rebuild everything
mvn clean install

# Just compile (no run)
mvn clean compile

# Run the app
mvn clean javafx:run

# Check database
mysql -u root -p
```

```sql
-- View users
SELECT * FROM user;

-- View products
SELECT * FROM barang LIMIT 5;

-- View transactions
SELECT * FROM transaksi;

-- View expenses
SELECT * FROM pengeluaran;
```

---

## ✨ Done? You Have:

✅ Database fully integrated
✅ User login working
✅ All data pre-loaded
✅ Authentication system ready
✅ DAOs for all entities
✅ Automatic calculations
✅ Complete documentation

---

## 🆘 Still Stuck?

1. Check **SETUP_CHECKLIST.md** - Step-by-step verification
2. Read **DATABASE_STRUCTURE.md** - Understand the schema
3. Review **README_INTEGRATION.md** - Full technical details
4. Check console output for error messages

---

**Everything is ready! 🎉**

Import database → Build project → Run app → Login

Good luck! 🚀
