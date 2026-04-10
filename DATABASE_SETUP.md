# Setup Koneksi Database dan User Authentication

## Deskripsi
Proyek PMITokoZikry menggunakan MySQL database dengan struktur tabel yang telah diintegrasikan. Sistem support multiple roles:
- **Kasir**: Akses untuk transaksi penjualan
- **Pemilik**: Akses untuk pengeluaran dan laporan

## Database Overview

### Struktur Tabel Utama
```
- user: Manajemen user login
- kategori: Kategori produk (ES_KRIM, MINUMAN, MAKANAN, PEMBERSIH, ROKOK, SEMBAKO, LAIN LAIN)
- barang: Data produk dengan stok, harga beli/jual
- transaksi: Riwayat transaksi penjualan
- detail_transaksi: Detail item dalam setiap transaksi (with triggers)
- pengeluaran: Pencatatan pengeluaran toko
```

## Langkah-langkah Setup

### 1. Pastikan MySQL Running
- Buka MySQL Server (XAMPP, WAMP, atau standalone MySQL)
- Database server harus berjalan di localhost:3306

### 2. Konfigurasi Database Connection
File: `src/main/java/config/koneksi.java`

Default configuration (sudah sesuai):
```java
String DB = "jdbc:mysql://localhost:3306/umkm";
String user = "root";
String pass = "";  // kosong jika tidak ada password
```

Jika berbeda, edit sesuai setting lokal Anda.

### 3. Setup Database & User Data
**Opsi A: PHPMyAdmin**
1. Buka http://localhost/phpmyadmin
2. Buat database baru bernama `umkm` (jika belum ada)
3. Import file `database_setup.sql` ke database

**Opsi B: MySQL Command Line**
```bash
mysql -u root -p < database_setup.sql
```

**Opsi C: MySQL Workbench**
1. Buka MySQL Workbench
2. File → Open SQL Script → Pilih `database_setup.sql`
3. Execute (Ctrl+Shift+Enter)

### 4. Verifikasi Data User
Jalankan query ini untuk cek data user:
```sql
USE umkm;
SELECT * FROM user;
```

Hasil yang diharapkan:
```
id_user  | user_password | role
---------|---------------|--------
KSR001   | EZAK123       | kasir
PMK001   | EZAK321       | pemilik
```

## User Login Credentials

| ID User | Password | Role | Deskripsi |
|---------|----------|------|-----------|
| `KSR001` | `EZAK123` | Kasir | Kasir Utama |
| `PMK001` | `EZAK321` | Pemilik | Pemilik Toko |

## Menjalankan Aplikasi
```bash
mvn clean javafx:run
```

## Database Triggers (Otomatis)

### 1. **trg_subtotal**
Menghitung subtotal secara otomatis saat insert detail_transaksi:
```
subtotal = jumlah * harga_satuan
```

### 2. **trg_update_total**
Update total transaksi setelah detail ditambahkan:
```
total = SUM(semua subtotal dalam transaksi)
```

### 3. **trg_kurangi_stok**
Mengurangi stok barang otomatis saat transaksi:
```
stok = stok - jumlah_terjual
```

## Data Kategori yang Tersedia
- `ES_KRIM`: Es Krim dan Frozen Treats
- `MINUMAN`: Minuman dan Soft Drinks
- `MAKANAN`: Makanan Ringan dan Snacks
- `PEMBERSIH`: Produk Pembersih dan Sanitasi
- `ROKOK`: Produk Rokok dan Tembakau
- `SEMBAKO`: Bahan Pokok Makanan
- `LAIN LAIN`: Produk Lainnya

## Data Barang yang Tersedia
Total 30 produk sudah ter-load dengan stok, harga beli, dan harga jual.

Contoh:
- PADDLEPOP (ESK001): Stok 10, Harga Jual 6000
- MINERAL (MIM001): Stok 30, Harga Jual 9000
- BERAS DIAMOND (SBK002): Stok 30, Harga Jual 346000

## Troubleshooting

### Koneksi Database Gagal
```
Error: com.mysql.cj.jdbc.exceptions.CommunicationsException
```
**Solusi:**
1. Pastikan MySQL server sudah running
2. Cek username/password di `koneksi.java`
3. Pastikan database `umkm` sudah ada
4. Test connection: `mysql -u root -p -h localhost`

### Login Gagal
```
✗ Login Gagal - User ID: KSR001
```
**Solusi:**
1. Verifikasi data di database: `SELECT * FROM user WHERE id_user = 'KSR001';`
2. Password case-sensitive - perhatikan besar/kecil huruf
3. Lihat console log untuk error details
4. Pastikan user sudah diinsert ke tabel

Credentials yang benar:
- KSR001 / EZAK123
- PMK001 / EZAK321

### Query Error - Unknown column
```
ERROR 1054: Unknown column 'username' in 'where clause'
```
**Solusi:**
- Pastikan menggunakan field `id_user` bukan `username`
- Field yang benar: `id_user`, `user_password`, `role`

## Security Notes (PENTING)
⚠️ Implementasi ini menggunakan **plain text password** untuk demo.

**Untuk Production, gunakan:**
- Password hashing (BCrypt, Argon2, PBKDF2)
- Jangan hardcode database credentials
- Environment variables untuk config
- HTTPS/SSL encryption
- Input validation & SQL injection prevention