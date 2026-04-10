# 🎯 Dashboard Separation - Kasir vs Pemilik

## Overview

The application now features role-based dashboard separation:
- **Kasir (Cashier)** - Transaction-focused interface for point-of-sale operations
- **Pemilik (Owner)** - Business analytics and management interface

## Architecture

### User Session Management

A singleton `UserSession` class maintains the logged-in user context:

```java
UserSession.getInstance().setCurrentUser(user);  // After login
UserSession.getInstance().getCurrentUser();       // Retrieve user
UserSession.getInstance().logout();               // Clear session
```

### Login Flow

```
LoginController
    ↓
UserDAO.validateUser(id, password)
    ↓ (User found)
UserSession.setCurrentUser(user)
    ↓
Check user.getRole()
    ├─ "kasir"      → KasirDashboardView.fxml
    └─ "pemilik"    → OwnerDashboardView.fxml
```

---

## Kasir (Cashier) Dashboard

### Features
- 🛒 **Transaction Entry** - Create new sales transactions
- 📦 **Product Lookup** - View inventory and available products
- ⚡ **Quick Checkout** - Add items and process sales
- 📊 **Daily Summary** - Quick stats for current session

### Interface Layout

```
┌─────────────────────────────────────────────────────────┐
│ User: KSR001 │ Role: KASIR              │ [Logout]      │ ← Top Bar
├─────────────────────────────────────────────────────────┤
│ [Transaksi] │ ┌──────────────────────────────────────┐ │
│ ─────────── │ │  ID Transaksi                        │ │
│             │ │  [_____________]                     │ │
│ [Produk]    │ │                                      │ │
│             │ │  Tanggal: [Date Picker]              │ │
│             │ │                                      │ │
│             │ │  Pilih Produk: [ComboBox v]          │ │
│             │ │  Jumlah: [Spinner: 1]                │ │
│             │ │                                      │ │
│             │ │  [Tambah] [Bersihkan] [Selesaikan]   │ │
│             │ └──────────────────────────────────────┘ │
├─────────────────────────────────────────────────────────┤
│ PMITokoZikry v1.0 - Kasir Dashboard        Status: Ready│ ← Status Bar
└─────────────────────────────────────────────────────────┘
```

### Tabs

#### 1. Transaksi (Transactions)
- Create new transaction
- Add items to transaction
- View transaction items
- Complete/finalize transaction

**Form Fields:**
- ID Transaksi (auto-generated or manual)
- Tanggal (date picker)
- Pilih Produk (dropdown from database)
- Jumlah (quantity spinner)

**Buttons:**
- Tambah Item (Add item to transaction)
- Bersihkan (Clear form)
- Selesaikan Transaksi (Complete transaction)

#### 2. Daftar Produk (Product List)
- View all available products
- Search/filter products
- View stock levels
- View selling prices

**Table Columns:**
- ID Barang
- Nama Barang
- Stok
- Harga Jual

---

## Pemilik (Owner) Dashboard

### Features
- 📈 **Sales Analytics** - Daily/weekly/monthly reports
- 💰 **Expense Management** - Track and categorize expenses
- 📦 **Inventory Control** - View all products and stock
- ⚙️ **System Settings** - Database backup, user management

### Interface Layout

```
┌─────────────────────────────────────────────────────────┐
│ User: PMK001 │ Role: PEMILIK             │ [Logout]     │ ← Top Bar
├──────────────┬───────────────────────────────────────────┤
│ [Laporan]    │ ┌────────────────────────────────────┐  │
│ ──────────   │ │ Laporan Penjualan & Analitik      │  │
│              │ │                                    │  │
│ [Pengeluaran]│ │ Periode: [Bulan Ini ▼]             │  │
│ ──────────   │ │                                    │  │
│              │ │ ┌────────────────────────────────┐ │  │
│ [Produk]     │ │ │ Total Penjualan: Rp 1.500.000  │ │  │
│ ──────────   │ │ │ Total Pengeluaran: Rp 456.000  │ │  │
│              │ │ │ Keuntungan: Rp 1.044.000       │ │  │
│ [Pengaturan] │ │ │ Transaksi: 12                  │ │  │
│              │ │ └────────────────────────────────┘ │  │
│              │ │                                    │  │
│              │ │ [Transaksi List...]               │  │
│              │ └────────────────────────────────────┘  │
├──────────────┴───────────────────────────────────────────┤
│ PMITokoZikry v1.0 - Pemilik Dashboard    Status: Ready  │ ← Status Bar
└─────────────────────────────────────────────────────────┘
```

### Tabs

#### 1. Laporan & Analitik (Reports & Analytics)
**Period Selection:**
- Hari Ini (Today)
- Minggu Ini (This Week)
- Bulan Ini (This Month)
- Tahun Ini (This Year)
- Custom Range

**Summary Cards (KPI Display):**
- Total Penjualan (Total Sales)
- Total Pengeluaran (Total Expenses)
- Keuntungan (Profit)
- Jumlah Transaksi (Transaction Count)

**Transaction History:**
- Table showing latest transactions
- Columns: ID, Tanggal, Total

#### 2. Pengeluaran (Expenses)
**Add Expense Form:**
- ID Pengeluaran
- Tanggal (Date)
- Nominal (Amount)
- Jenis (Type: PLN, Air, Plastik, dll)
- [Tambah] Button

**Expenses Table:**
- Columns: ID, Tanggal, Jenis, Nominal
- Shows all recorded expenses
- Sortable and filterable

#### 3. Produk (Products)
**Product Inventory Table:**
- Columns: ID Barang, Nama Barang, Stok, Harga Beli, Harga Jual
- View all products
- Monitor stock levels
- Track profit margins

#### 4. Pengaturan (Settings)
**Database & System:**
- Backup Database Button
- Export Data Button

**User Management:**
- Kelola User (Manage Users)
- Ubah Password (Change Password)

---

## File Structure

```
src/main/java/
├── config/
│   ├── koneksi.java           (Database connection)
│   └── UserSession.java       (Session management) ← NEW
│
├── Controller/
│   ├── LoginController.java   (Updated with role routing)
│   ├── KasirDashboardController.java      ← NEW
│   ├── OwnerDashboardController.java      ← NEW
│   └── ... other controllers
│
src/main/resources/
└── FXML/
    ├── LoginView.fxml
    ├── KasirDashboardView.fxml      ← NEW
    ├── OwnerDashboardView.fxml      ← NEW
    └── ... other views
```

---

## Implementation Details

### 1. UserSession Singleton

```java
// Store user after login
UserSession.getInstance().setCurrentUser(user);

// Check if user is logged in
if (UserSession.getInstance().isLoggedIn()) {
    // User is logged in
}

// Get current user
User user = UserSession.getInstance().getCurrentUser();

// Check role
if (UserSession.getInstance().isKasir()) {
    // Kasir-specific actions
}

// Logout
UserSession.getInstance().logout();
```

### 2. LoginController Routing

```java
// After successful login validation
User user = UserDAO.validateUser(userId, password);
UserSession.getInstance().setCurrentUser(user);

// Route based on role
if ("kasir".equalsIgnoreCase(user.getRole())) {
    // Load KasirDashboardView.fxml
} else if ("pemilik".equalsIgnoreCase(user.getRole())) {
    // Load OwnerDashboardView.fxml
}
```

### 3. KasirDashboardController

Key methods:
- `initialize()` - Initialize UI and load tabs
- `loadTransaksiTab()` - Create transaction entry interface
- `loadBarangTab()` - Display product list
- `handleLogout()` - Clear session and return to login

### 4. OwnerDashboardController

Key methods:
- `initialize()` - Initialize UI and load tabs
- `loadLaporanTab()` - Display analytics and reports
- `loadPengeluaranTab()` - Display expense management
- `loadBarangTab()` - Display inventory
- `loadPengaturanTab()` - Display settings
- `handleLogout()` - Clear session and return to login

---

## FXML Files

### KasirDashboardView.fxml
- BorderPane layout with top bar, sidebar, tabs, status bar
- Top bar: User info + Logout button (Blue - #2196F3)
- Sidebar: Transaction and Product buttons
- Main content: TabPane with 2 tabs
- Status bar: Application info

### OwnerDashboardView.fxml
- BorderPane layout with top bar, sidebar, tabs, status bar
- Top bar: User info + Logout button (Orange - #FF9800)
- Sidebar: 4 buttons (Laporan, Pengeluaran, Produk, Pengaturan)
- Main content: TabPane with 4 tabs
- Status bar: Application info

---

## Login Testing

**Test Credentials:**

| User ID | Password | Role | Dashboard |
|---------|----------|------|-----------|
| KSR001 | EZAK123 | kasir | Blue - Transaction-focused |
| PMK001 | EZAK321 | pemilik | Orange - Management-focused |

---

## Error Handling

Both dashboards include try-catch blocks for:
- FXML loading errors
- Database connection issues
- Tab initialization failures
- Logout errors

Console logging for debugging:
```
[KasirDashboard] Initialized for user: KSR001
[KasirDashboard] Loading Transaksi Tab...
[KasirDashboard] Barang Tab loaded with 30 items
```

---

## Future Enhancements

### Kasir Dashboard
- [ ] Real transaction processing
- [ ] Payment method selection
- [ ] Receipt printing
- [ ] Discount application
- [ ] Daily closing report
- [ ] Quick transaction history

### Owner Dashboard
- [ ] Advanced analytics and charts
- [ ] Profit margin calculations
- [ ] Inventory alerts (low stock)
- [ ] User activity logs
- [ ] Data export (PDF/Excel)
- [ ] Multi-user sales comparison

### Common
- [ ] Role-based permission system
- [ ] Audit logging
- [ ] Password change functionality
- [ ] Session timeout handling
- [ ] Menu customization per role

---

## Building & Running

```bash
# Build project
mvn clean compile

# Run application
mvn clean javafx:run

# Login with:
# Kasir: KSR001 / EZAK123
# Pemilik: PMK001 / EZAK321
```

---

## Color Scheme

| Role | Color | Hex | Purpose |
|------|-------|-----|---------|
| Kasir | Blue | #2196F3 | Transaction/Sales |
| Pemilik | Orange | #FF9800 | Management/Analytics |

---

**Status**: ✅ Complete - Both dashboards fully implemented and integrated!
