# 📊 Database Structure Guide

## Table Relationships Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                         UMKM DATABASE                            │
└─────────────────────────────────────────────────────────────────┘

┌──────────────┐          ┌──────────────────┐
│    user      │          │    kategori      │
├──────────────┤          ├──────────────────┤
│ id_user (PK) │◄─┐       │ id_kategori (PK) │
│ password     │  │       │ nama_kategori    │
│ role         │  │       └──────────────────┘
└──────────────┘  │                 ▲
                  │                 │ FK
                  │                 │
                  │       ┌─────────┴──────────┐
                  │       │                    │
                  │   ┌───────────┐    ┌──────────────┐
                  │   │  barang   │    │ pengeluaran  │
                  │   ├───────────┤    ├──────────────┤
                  │   │ id_barang │    │ id_pengel(PK)│
                  │   │ nama      │    │ tgl          │
                  │   │ stok      │    │ nominal      │
                  │   │ harga_beli│    │ jenis        │
                  │   │ harga_jual│    │ id_user(FK)◄─┼─┐
                  │   └─────┬─────┘    └──────────────┘ │
                  │         │                           │
                  │         │ FK (id_barang)            │
                  │         │                           │
                  │   ┌─────▼──────────────────┐        │
                  │   │ detail_transaksi       │        │
                  │   ├────────────────────────┤        │
                  │   │ id_detail (PK)         │        │
                  │   │ id_transaksi (FK)  ────┼────────┼──┐
                  │   │ id_barang (FK)     ────┼────────┘  │
                  │   │ jumlah                 │           │
                  │   │ harga_satuan           │           │
                  │   │ subtotal               │           │
                  │   └────────────────────────┘           │
                  │                                       │
                  └───┬────────────────────────────────────┘
                      │
                  ┌───▼──────────────┐
                  │   transaksi      │
                  ├──────────────────┤
                  │ id_transaksi(PK) │
                  │ tgl              │
                  │ id_user(FK)      │
                  │ total            │
                  └──────────────────┘
```

---

## Table Details

### 1. USER (User Authentication)

```sql
CREATE TABLE user (
  id_user VARCHAR(20) PRIMARY KEY,         -- Format: USR###
  user_password VARCHAR(255) NOT NULL,    -- Encrypted password
  role ENUM('kasir','pemilik') NOT NULL   -- User role
);
```

**Records**:
| id_user | user_password | role |
|---------|---------------|------|
| KSR001 | EZAK123 | kasir |
| PMK001 | EZAK321 | pemilik |

**Purpose**: Store user credentials and roles
**Access**: All other tables reference this for user tracking

---

### 2. KATEGORI (Product Categories)

```sql
CREATE TABLE kategori (
  id_kategori VARCHAR(20) PRIMARY KEY,     -- Format: CAT000
  nama_kategori VARCHAR(100) NOT NULL,    -- Category name
  UNIQUE INDEX nama_kategori (nama_kategori)
);
```

**Records** (7 categories):
| id_kategori | nama_kategori |
|-------------|---------------|
| ESK000 | ES_KRIM |
| MIM000 | MINUMAN |
| MKM000 | MAKANAN |
| PBR000 | PEMBERSIH |
| RKK000 | ROKOK |
| SBK000 | SEMBAKO |
| DLL000 | LAIN LAIN |

**Purpose**: Organize products into categories

---

### 3. BARANG (Product Inventory)

```sql
CREATE TABLE barang (
  id_barang VARCHAR(20) PRIMARY KEY,       -- Format: PROD###
  nama_barang VARCHAR(100) NOT NULL,      -- Product name
  id_kategori VARCHAR(20),                 -- FK to kategori
  stok INT DEFAULT 0,                      -- Current stock
  harga_beli DECIMAL(12,2) NOT NULL,      -- Cost price
  harga_jual DECIMAL(12,2) NOT NULL,      -- Selling price
  FOREIGN KEY (id_kategori) REFERENCES kategori(id_kategori)
    ON DELETE SET NULL ON UPDATE CASCADE,
  INDEX idx_barang_kategori (id_kategori)
);
```

**Sample Records** (30 products total):
| id_barang | nama_barang | id_kategori | stok | harga_beli | harga_jual |
|-----------|-------------|------------|------|------------|-----------|
| ESK001 | PADDLEPOP | ESK000 | 10 | 5000.00 | 6000.00 |
| MIM001 | MINERAL | MIM000 | 30 | 8000.00 | 9000.00 |
| SBK002 | BERAS DIAMOND | SBK000 | 30 | 300000.00 | 346000.00 |

**Purpose**: Maintain product inventory

---

### 4. TRANSAKSI (Sales Transactions)

```sql
CREATE TABLE transaksi (
  id_transaksi VARCHAR(20) PRIMARY KEY,    -- Format: TRK####
  tgl_transaksi DATETIME DEFAULT NOW(),   -- Transaction date/time
  id_user VARCHAR(20),                     -- FK to user
  total DECIMAL(12,2) DEFAULT 0,          -- Total amount
  FOREIGN KEY (id_user) REFERENCES user(id_user)
    ON DELETE SET NULL ON UPDATE CASCADE,
  INDEX idx_transaksi_user (id_user)
);
```

**Sample Records**:
| id_transaksi | tgl_transaksi | id_user | total |
|-------------|--------------|---------|-------|
| TRK001 | 2026-04-01 22:13:37 | KSR001 | 300000.00 |

**Purpose**: Record each sale transaction

---

### 5. DETAIL_TRANSAKSI (Transaction Line Items)

```sql
CREATE TABLE detail_transaksi (
  id_detail VARCHAR(20) PRIMARY KEY,       -- Format: DTL####
  id_transaksi VARCHAR(20) NOT NULL,       -- FK to transaksi
  id_barang VARCHAR(20),                   -- FK to barang
  jumlah INT NOT NULL,                     -- Quantity
  harga_satuan DECIMAL(12,2) NOT NULL,    -- Unit price
  subtotal DECIMAL(12,2),                  -- Line total (auto-calc)
  FOREIGN KEY (id_transaksi) REFERENCES transaksi(id_transaksi)
    ON DELETE CASCADE ON UPDATE CASCADE,
  FOREIGN KEY (id_barang) REFERENCES barang(id_barang)
    ON DELETE SET NULL ON UPDATE CASCADE,
  INDEX idx_detail_transaksi (id_transaksi),
  INDEX idx_detail_barang (id_barang)
);
```

**Sample Records**:
| id_detail | id_transaksi | id_barang | jumlah | harga_satuan | subtotal |
|----------|-------------|-----------|--------|-------------|----------|
| DTL001 | TRK001 | ESK005 | 60 | 5000.00 | 300000.00 |

**Purpose**: Store individual items in a transaction

---

### 6. PENGELUARAN (Expense Tracking)

```sql
CREATE TABLE pengeluaran (
  id_pengeluaran VARCHAR(20) PRIMARY KEY,  -- Format: PGN####
  tgl_pengeluaran DATE NOT NULL,          -- Expense date
  nominal DECIMAL(12,2) NOT NULL,         -- Amount
  jenis VARCHAR(100),                      -- Type (PLN, AIR, etc)
  id_user VARCHAR(20),                     -- FK to user
  FOREIGN KEY (id_user) REFERENCES user(id_user)
    ON DELETE SET NULL ON UPDATE CASCADE,
  INDEX id_user (id_user)
);
```

**Sample Records**:
| id_pengeluaran | tgl_pengeluaran | nominal | jenis | id_user |
|---------------|-----------------|---------|-------|---------|
| PGN001 | 2026-04-01 | 100000.00 | PLN | PMK001 |
| PGN002 | 2026-04-01 | 326000.00 | AIR | PMK001 |
| PGN003 | 2026-04-01 | 30000.00 | PLASTIK KATONGAN | PMK001 |

**Purpose**: Track business expenses

---

## Database Triggers

### Trigger 1: trg_subtotal (BEFORE INSERT on detail_transaksi)

```sql
BEFORE INSERT ON detail_transaksi
BEGIN
  SET NEW.subtotal = NEW.jumlah * NEW.harga_satuan;
END
```

**Purpose**: Automatically calculate subtotal when detail is added

---

### Trigger 2: trg_update_total (AFTER INSERT on detail_transaksi)

```sql
AFTER INSERT ON detail_transaksi
BEGIN
  UPDATE transaksi
  SET total = (
    SELECT SUM(subtotal)
    FROM detail_transaksi
    WHERE id_transaksi = NEW.id_transaksi
  )
  WHERE id_transaksi = NEW.id_transaksi;
END
```

**Purpose**: Update transaction total when item added

---

### Trigger 3: trg_kurangi_stok (AFTER INSERT on detail_transaksi)

```sql
AFTER INSERT ON detail_transaksi
BEGIN
  UPDATE barang
  SET stok = stok - NEW.jumlah
  WHERE id_barang = NEW.id_barang;
END
```

**Purpose**: Reduce stock when item is sold

---

## Data Types Used

| Type | Purpose | Examples |
|------|---------|----------|
| VARCHAR(20) | ID fields | id_user, id_barang |
| VARCHAR(100) | Names/descriptions | nama_barang, jenis |
| INT | Whole numbers | stok, jumlah |
| DECIMAL(12,2) | Money amounts | harga_beli, nominal |
| DATETIME | Date + time | tgl_transaksi |
| DATE | Date only | tgl_pengeluaran |
| ENUM | Fixed choices | role ('kasir','pemilik') |

---

## Relationships

### Foreign Key Relationships

1. **user ← pengeluaran**
   - A user can have many expenses
   - Delete user → expenses stay but user_id becomes NULL

2. **user ← transaksi**
   - A user can have many transactions
   - Delete user → transactions stay but user_id becomes NULL

3. **kategori ← barang**
   - A category can have many products
   - Delete category → products stay but category_id becomes NULL

4. **barang ← detail_transaksi**
   - A product can be in many transactions
   - Delete product → details stay but product_id becomes NULL

5. **transaksi ← detail_transaksi**
   - A transaction has many items
   - Delete transaction → CASCADE delete all items

---

## Index Strategy

| Table | Index | Columns | Purpose |
|-------|-------|---------|---------|
| kategori | nama_kategori | nama_kategori | Fast category lookup by name |
| barang | idx_barang_kategori | id_kategori | Fast product search by category |
| transaksi | idx_transaksi_user | id_user | Fast user transaction lookup |
| detail_transaksi | idx_detail_transaksi | id_transaksi | Fast item lookup in transaction |
| detail_transaksi | idx_detail_barang | id_barang | Fast product usage tracking |
| pengeluaran | id_user | id_user | Fast expense lookup by user |

---

## Sample Data Summary

| Table | Records | Notes |
|-------|---------|-------|
| user | 2 | KSR001, PMK001 |
| kategori | 7 | All categories loaded |
| barang | 30 | Complete inventory |
| transaksi | 1 | Sample transaction |
| detail_transaksi | 1 | Sample item (60 units sold) |
| pengeluaran | 3 | Sample expenses |

---

## Queries for Common Tasks

**List all products**
```sql
SELECT b.id_barang, b.nama_barang, k.nama_kategori, 
       b.stok, b.harga_jual
FROM barang b
LEFT JOIN kategori k ON b.id_kategori = k.id_kategori
ORDER BY k.nama_kategori, b.nama_barang;
```

**Get user's sales**
```sql
SELECT t.id_transaksi, t.tgl_transaksi, t.total,
       COUNT(dt.id_detail) as items
FROM transaksi t
LEFT JOIN detail_transaksi dt ON t.id_transaksi = dt.id_transaksi
WHERE t.id_user = 'KSR001'
GROUP BY t.id_transaksi
ORDER BY t.tgl_transaksi DESC;
```

**Get low stock items**
```sql
SELECT id_barang, nama_barang, stok
FROM barang
WHERE stok < 10
ORDER BY stok ASC;
```

**Get daily sales total**
```sql
SELECT DATE(tgl_transaksi) as tanggal, COUNT(*) as transaksi,
       SUM(total) as total_penjualan
FROM transaksi
GROUP BY DATE(tgl_transaksi)
ORDER BY tanggal DESC;
```

**Get total expenses by type**
```sql
SELECT jenis, SUM(nominal) as total
FROM pengeluaran
WHERE MONTH(tgl_pengeluaran) = MONTH(CURDATE())
GROUP BY jenis;
```

---

**Database Design Complete ✅**
