# 🚀 Dashboard Separation - Quick Reference

## Login & Dashboard Routing

### Kasir (Cashier)
```
Login Screen
    ↓
Username: KSR001
Password: EZAK123
    ↓
UserSession.setCurrentUser(user)
    ↓
Route to: KasirDashboardView.fxml
    ↓
🔵 BLUE Dashboard - Transaction Focus
```

### Pemilik (Owner)
```
Login Screen
    ↓
Username: PMK001
Password: EZAK321
    ↓
UserSession.setCurrentUser(user)
    ↓
Route to: OwnerDashboardView.fxml
    ↓
🟠 ORANGE Dashboard - Management Focus
```

---

## Dashboard Features at a Glance

### Kasir Dashboard 🔵
| Feature | Access | Purpose |
|---------|--------|---------|
| Transaction Entry | Transaksi Tab | Create new sales |
| Product Selection | Transaksi Tab | Choose items to sell |
| Inventory View | Daftar Produk Tab | Check stock levels |
| User Display | Top Bar | Show logged-in user |
| Logout | Top Bar Button | Exit and return to login |

### Pemilik Dashboard 🟠
| Feature | Access | Purpose |
|---------|--------|---------|
| Sales Report | Laporan Tab | View analytics & KPIs |
| Add Expense | Pengeluaran Tab | Record business costs |
| View Expenses | Pengeluaran Tab | See expense history |
| Product Inventory | Produk Tab | Monitor stock & pricing |
| System Settings | Pengaturan Tab | Manage system & users |
| User Display | Top Bar | Show logged-in user |
| Logout | Top Bar Button | Exit and return to login |

---

## File Structure

```
New/Modified Files:
├── config/
│   └── UserSession.java .................... Session management
├── Controller/
│   ├── LoginController.java ................ Updated with routing
│   ├── KasirDashboardController.java ....... NEW - Kasir logic
│   └── OwnerDashboardController.java ....... NEW - Pemilik logic
└── resources/FXML/
    ├── KasirDashboardView.fxml ............ NEW - Kasir UI
    └── OwnerDashboardView.fxml ............ NEW - Pemilik UI
```

---

## Key Classes

### UserSession (Singleton)
```java
UserSession.getInstance().setCurrentUser(user);    // Set user
UserSession.getInstance().getCurrentUser();        // Get user
UserSession.getInstance().isKasir();              // Check if cashier
UserSession.getInstance().isPemilik();            // Check if owner
UserSession.getInstance().logout();               // Clear session
UserSession.getInstance().isLoggedIn();           // Check login status
```

### KasirDashboardController
```java
initialize()           // Setup UI and load tabs
loadTransaksiTab()     // Create transaction interface
loadBarangTab()        // Display product list
handleLogout()         // Logout handler
```

### OwnerDashboardController
```java
initialize()           // Setup UI and load tabs
loadLaporanTab()       // Create analytics dashboard
loadPengeluaranTab()   // Create expense manager
loadBarangTab()        // Display inventory
loadPengaturanTab()    // Create settings panel
handleLogout()         // Logout handler
```

---

## Build & Run

```bash
# 1. Build
mvn clean compile

# 2. Run
mvn clean javafx:run

# 3. Test Kasir Login
#    Username: KSR001
#    Password: EZAK123
#    ✅ Should see blue dashboard

# 4. Test Owner Login
#    Username: PMK001
#    Password: EZAK321
#    ✅ Should see orange dashboard
```

---

## UI Styling

### Kasir Dashboard (Blue)
```
Top Bar:        #2196F3 (Blue)
Sidebar:        #f5f5f5 (Light Gray)
Tab Labels:     Transaksi, Daftar Produk
Window Title:   "PMITokoZikry - kasir"
```

### Pemilik Dashboard (Orange)
```
Top Bar:        #FF9800 (Orange)
Sidebar:        #f5f5f5 (Light Gray)
Tab Labels:     Laporan & Analitik, Pengeluaran, Produk, Pengaturan
Window Title:   "PMITokoZikry - pemilik"
```

---

## Common Tasks

### For Developers

**Add new feature to Kasir Dashboard**
```java
// In KasirDashboardController
@FXML
private Tab newFeatureTab;  // Add FXML reference

private void loadNewFeatureTab() {
    // Create UI elements
    VBox vbox = new VBox(10);
    // ... add content
    newFeatureTab.setContent(vbox);
}
```

**Add new feature to Pemilik Dashboard**
```java
// In OwnerDashboardController
@FXML
private Tab newFeatureTab;  // Add FXML reference

private void loadNewFeatureTab() {
    // Create UI elements
    VBox vbox = new VBox(10);
    // ... add content
    newFeatureTab.setContent(vbox);
}
```

### For Users

**As Kasir**
1. Login with KSR001/EZAK123
2. Go to Transaksi tab
3. Fill in transaction details
4. Add products and click "Selesaikan Transaksi"
5. Click Logout to exit

**As Pemilik**
1. Login with PMK001/EZAK321
2. View reports in Laporan tab
3. Add expenses in Pengeluaran tab
4. Check inventory in Produk tab
5. Access settings in Pengaturan tab
6. Click Logout to exit

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Wrong dashboard loads | Check UserSession login storage |
| Layout looks broken | Verify FXML syntax in IDE |
| Cannot compile | Run `mvn clean install` |
| Database error | Check koneksi.java config |
| Logout not working | Verify logout handler implementation |

---

## Documentation Files

- **DASHBOARD_SEPARATION.md** - Complete detailed guide
- **DASHBOARD_IMPLEMENTATION.md** - Implementation summary
- **README_INTEGRATION.md** - Full project reference
- **QUICKSTART.md** - Quick setup guide

---

## Version Info

| Component | Version | Status |
|-----------|---------|--------|
| Java | 9+ | ✅ |
| JavaFX | 21.0.6 | ✅ |
| MySQL | 8.0.30 | ✅ |
| Project | 1.0 | ✅ |

---

## Quick Stats

- **Controllers**: 2 new dashboard controllers
- **FXML Views**: 2 new dashboard views
- **Session Class**: 1 singleton
- **Modified Controllers**: 1 (LoginController)
- **Lines of Code**: ~1500+ (controllers)
- **Documentation**: 2 new guides

---

## Status: ✅ COMPLETE

All dashboard separation tasks completed and ready for testing!

🎉 **Ready to Deploy!**
