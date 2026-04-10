# ✅ Testing Guide - Dashboard Separation

## Step-by-Step Test Process

### Phase 1: Build & Compile (5 minutes)

**Step 1.1: Clean Build**
```bash
cd C:\Users\MSi-GAMING\IdeaProjects\PMITokoZikry
mvn clean
```
Expected: Removes target directory

**Step 1.2: Compile Project**
```bash
mvn compile
```
Expected output:
```
[INFO] BUILD SUCCESS
```

If you get errors, check:
- All import statements are present
- No typos in class names
- Database is running

---

### Phase 2: Verify Database (2 minutes)

**Step 2.1: Check Database Connection**
```bash
mysql -u root -p
```
(Enter password or just press Enter)

**Step 2.2: Verify User Table**
```sql
USE umkm;
SELECT * FROM user;
```

Expected output:
```
+--------+----------+--------+
| id_user| password | role   |
+--------+----------+--------+
| KSR001 | EZAK123  | kasir  |
| PMK001 | EZAK321  | pemilik|
+--------+----------+--------+
```

If not present, re-run: `mysql -u root -p < database_setup.sql`

---

### Phase 3: Run Application (3 minutes)

**Step 3.1: Start Application**
```bash
mvn clean javafx:run
```

**Step 3.2: Wait for Window**
- Application should open in ~10-15 seconds
- Login form should display

**Expected Screen:**
```
╔═══════════════════════════════╗
║   PMITokoZikry - Login        ║
║                               ║
║  Username: [_____________]    ║
║  Password: [_____________]    ║
║                               ║
║  [Login Button]               ║
│                               │
║ (Error message if needed)     ║
╚═══════════════════════════════╝
```

---

### Phase 4: Test Kasir Dashboard (4 minutes)

**Step 4.1: Login as Kasir**
1. Username: `KSR001`
2. Password: `EZAK123`
3. Click "Login"

**Expected Result:**
✅ Blue dashboard window opens
✅ Window title: "PMITokoZikry - kasir"
✅ Top bar shows: "User: KSR001" and "Role: KASIR"
✅ Sidebar buttons visible: "Transaksi", "Produk"
✅ Two tabs visible: "Transaksi", "Daftar Produk"

**Step 4.2: Test Transaksi Tab**
1. Click "Transaksi" tab
2. Should see:
   - ✅ ID Transaksi field
   - ✅ Tanggal date picker
   - ✅ Pilih Produk dropdown
   - ✅ Jumlah spinner
   - ✅ Action buttons (Tambah, Bersihkan, Selesaikan)

**Step 4.3: Test Daftar Produk Tab**
1. Click "Daftar Produk" tab
2. Should see:
   - ✅ Search/filter field
   - ✅ Product table with columns:
     - ID Barang
     - Nama Barang
     - Stok
     - Harga Jual
   - ✅ Should show 30 products from database

**Step 4.4: Test Logout**
1. Click "Logout" button
2. Should return to login screen
3. ✅ Session cleared

---

### Phase 5: Test Pemilik Dashboard (4 minutes)

**Step 5.1: Login as Pemilik**
1. Username: `PMK001`
2. Password: `EZAK321`
3. Click "Login"

**Expected Result:**
✅ Orange dashboard window opens
✅ Window title: "PMITokoZikry - pemilik"
✅ Top bar shows: "User: PMK001" and "Role: PEMILIK"
✅ Sidebar buttons visible: "Laporan", "Pengeluaran", "Produk", "Pengaturan"
✅ Four tabs visible

**Step 5.2: Test Laporan & Analitik Tab**
1. Click "Laporan & Analitik" tab
2. Should see:
   - ✅ Period selector dropdown
   - ✅ Summary cards:
     - Total Penjualan (colored with border)
     - Total Pengeluaran (colored with border)
     - Keuntungan (colored with border)
     - Jumlah Transaksi (colored with border)
   - ✅ Transaction list table

**Step 5.3: Test Pengeluaran Tab**
1. Click "Pengeluaran" tab
2. Should see:
   - ✅ Add expense form with fields:
     - ID Pengeluaran
     - Tanggal (date picker)
     - Nominal (amount)
     - Jenis (type)
     - Tambah button
   - ✅ Expenses table with data from database

**Step 5.4: Test Produk Tab**
1. Click "Produk" tab
2. Should see:
   - ✅ Inventory table with 30 products
   - ✅ Columns: ID Barang, Nama Barang, Stok, Harga Beli, Harga Jual

**Step 5.5: Test Pengaturan Tab**
1. Click "Pengaturan" tab
2. Should see:
   - ✅ Database & Sistem section with buttons
   - ✅ Manajemen User section with buttons

**Step 5.6: Test Logout**
1. Click "Logout" button
2. Should return to login screen
3. ✅ Session cleared

---

### Phase 6: Cross-Test (3 minutes)

**Step 6.1: Logout and Relogin**
1. Logout from current dashboard
2. Login with opposite user (if was Kasir, login as Pemilik and vice versa)
3. ✅ Should load correct dashboard with correct color

**Step 6.2: Wrong Credentials**
1. Try login with wrong password
2. ✅ Should show error: "Username atau Password salah!"
3. Login box remains open

**Step 6.3: Empty Fields**
1. Try login without entering anything
2. ✅ Should show error: "Username dan Password tidak boleh kosong!"

---

## Console Output to Expect

When everything works, you should see console logs like:

```
[UserSession] User logged in: KSR001 (kasir)
[KasirDashboard] Initialized for user: KSR001
[KasirDashboard] Loading Transaksi Tab...
[KasirDashboard] Loading Barang Tab...
[KasirDashboard] Barang Tab loaded with 30 items
✓ Navigasi ke dashboard: /FXML/KasirDashboardView.fxml
```

Or for Pemilik:

```
[UserSession] User logged in: PMK001 (pemilik)
[OwnerDashboard] Initialized for user: PMK001
[OwnerDashboard] Loading Laporan Tab...
[OwnerDashboard] Loading Pengeluaran Tab...
[OwnerDashboard] Barang Tab loaded with 30 items
✓ Navigasi ke dashboard: /FXML/OwnerDashboardView.fxml
```

---

## Test Results Checklist

### Compilation Phase
- [ ] `mvn clean` completes without errors
- [ ] `mvn compile` completes with BUILD SUCCESS
- [ ] No import errors in IDE

### Database Phase
- [ ] Can connect to MySQL
- [ ] Database `umkm` exists
- [ ] `user` table has 2 records
- [ ] `barang` table has 30 records
- [ ] `kategori` table has 7 records

### Kasir Dashboard Phase
- [ ] Login with KSR001/EZAK123 succeeds
- [ ] Blue dashboard loads
- [ ] Window title shows "kasir"
- [ ] User info displays correctly
- [ ] Both tabs load without errors
- [ ] Product table shows 30 items
- [ ] Logout button works

### Pemilik Dashboard Phase
- [ ] Login with PMK001/EZAK321 succeeds
- [ ] Orange dashboard loads
- [ ] Window title shows "pemilik"
- [ ] User info displays correctly
- [ ] All 4 tabs load without errors
- [ ] Analytics cards display
- [ ] Expense table shows data
- [ ] Logout button works

### Cross-Testing Phase
- [ ] Can logout and login with different user
- [ ] Wrong password shows error
- [ ] Empty fields show error
- [ ] Session is properly cleared on logout
- [ ] Window title changes with each login
- [ ] Colors change with each login (blue/orange)

---

## Final Status Check

✅ **All tests passed?** → Application is ready!
❌ **Some tests failed?** → Check FIXES_APPLIED.md for solutions

---

## Estimated Time

| Phase | Time | Status |
|-------|------|--------|
| Build & Compile | 5 min | ⏱️ |
| Database Verify | 2 min | ⏱️ |
| Run Application | 3 min | ⏱️ |
| Test Kasir | 4 min | ⏱️ |
| Test Pemilik | 4 min | ⏱️ |
| Cross-Test | 3 min | ⏱️ |
| **TOTAL** | **~20 min** | ⏱️ |

---

## If Something Fails

1. **Check the console output** - Look for error messages
2. **Verify database connection** - Run `mysql -u root -p` test
3. **Check file paths** - Verify FXML files exist
4. **Review imports** - Check all imports are present
5. **Clean rebuild** - Run `mvn clean install`

---

## Success Criteria

The application is working correctly when:

✅ Kasir login shows blue dashboard
✅ Pemilik login shows orange dashboard
✅ Each dashboard has correct tabs
✅ Data loads from database
✅ Logout returns to login
✅ No errors in console
✅ All tests pass

---

**Testing Date**: 2026-04-10
**Expected Duration**: ~20 minutes
**Status**: Ready to test

🚀 **Let's test it!** 🚀
