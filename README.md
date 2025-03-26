# 📌 **Kompilasi Laporan Praktikum Sistem Manajemen Basis Data**

> **Penulis:** Faridatul Kasanah  
> **Mata Kuliah:** Sistem Manajemen Basis Data  
> **Dosen Pengampu:** Fadelis Sukya, S.Kom, M.Cs  
> **Institusi:** PSDKU Politeknik Negeri Malang di Kota Kediri  

---

## **📌 Pendahuluan**

Dalam era digital, manajemen basis data menjadi aspek penting dalam pengelolaan informasi. MySQL adalah salah satu sistem manajemen basis data relasional (RDBMS) yang banyak digunakan. Laporan ini merupakan hasil kompilasi berbagai praktikum yang telah dilakukan, mencakup **instalasi MySQL, manajemen user & role, optimasi query dengan indexing, bottleneck dalam MySQL, dan partisi tabel**. 

Setiap eksperimen ini bertujuan untuk memahami bagaimana mengelola dan mengoptimalkan database agar lebih efisien dan cepat dalam menangani query yang kompleks.

---

# **1️⃣ Instalasi dan Konfigurasi MySQL**

## **📌 Latar Belakang**
MySQL adalah sistem manajemen basis data yang digunakan dalam berbagai aplikasi, dari skala kecil hingga enterprise. Instalasi yang baik dan konfigurasi yang tepat akan meningkatkan kinerja dan keamanan database.

## **📌 Problem yang Dihadapi**
- Instalasi default memiliki parameter yang kurang optimal.
- Perubahan port MySQL dari **3306** ke **3309**.
- Menyesuaikan `innodb_buffer_pool_size` agar lebih efisien.
- Mengubah password root untuk meningkatkan keamanan.

## **📌 Solusi dan Implementasi**

### **🔹 Instalasi MySQL**
1. **Download MySQL Installer** dari [situs resmi](https://dev.mysql.com/downloads/installer/).
2. **Jalankan Installer** dan ikuti proses instalasi.
3. **Pilih Server dan Workbench** untuk instalasi.
4. **Konfigurasi Root Password** dan simpan dengan aman.
5. **Selesaikan Instalasi dan Tes Koneksi** dengan MySQL Workbench atau Command Line.

### **🔹 Konfigurasi Awal**
#### **🔹 Mengubah Port MySQL**
```sql
# Buka file konfigurasi my.ini
[mysqld]
port=3309
```

#### **🔹 Mengatur Buffer Pool Size**
```sql
innodb_buffer_pool_size = 2G  # Jika RAM 8GB
```

#### **🔹 Mengubah Password Root**
```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'newpassword';
```

### **📌 Kesimpulan**
Konfigurasi yang optimal membantu meningkatkan performa dan keamanan database MySQL, terutama dalam skenario produksi.

---

# **2️⃣ Manajemen User, Role, dan Privilege di MySQL**

## **📌 Latar Belakang**
Manajemen user sangat penting untuk mengontrol akses database dan mencegah manipulasi data oleh pengguna yang tidak sah.

## **📌 Problem yang Dihadapi**
- Membuat user dengan akses terbatas.
- Memberikan role tertentu agar lebih mudah dalam manajemen hak akses.
- Menghapus atau mencabut akses user yang tidak diperlukan.

## **📌 Solusi dan Implementasi**

### **🔹 Membuat User**
```sql
CREATE USER 'user1'@'localhost' IDENTIFIED BY 'password1';
```

### **🔹 Membuat Role**
```sql
CREATE ROLE 'role_read_only';
GRANT SELECT ON database_name.* TO 'role_read_only';
```

### **🔹 Memberikan Role ke User**
```sql
GRANT 'role_read_only' TO 'user1'@'localhost';
```

### **🔹 Menghapus User**
```sql
DROP USER 'user1'@'localhost';
```

### **📌 Kesimpulan**
Dengan sistem role, pengelolaan hak akses menjadi lebih fleksibel dan aman, terutama dalam sistem multi-user.

---

# **3️⃣ Optimasi Bottleneck di MySQL**

## **📌 Latar Belakang**
Bottleneck dalam MySQL dapat menyebabkan kinerja database menjadi lambat dan tidak efisien. Identifikasi dan perbaikan bottleneck sangat penting.

## **📌 Problem yang Dihadapi**
- **Full Table Scan** karena tidak ada indeks.
- **Banyak koneksi terbuka** menyebabkan error `Too many connections`.
- **Deadlock dalam transaksi**.
- **Ukuran InnoDB Buffer Pool yang kecil**.

## **📌 Solusi dan Implementasi**

### **🔹 Menambahkan Indeks**
```sql
CREATE INDEX idx_customer_id ON orders(customer_id);
```

### **🔹 Menggunakan Query Cache** *(jika didukung oleh versi MySQL)*
```sql
SET GLOBAL query_cache_size = 64M;
```

### **🔹 Meningkatkan Buffer Pool Size**
```sql
innodb_buffer_pool_size = 5G;
```

### **📌 Kesimpulan**
Optimasi bottleneck dapat meningkatkan performa query dan mengurangi waktu eksekusi database secara signifikan.

---

# **4️⃣ Optimasi Database dengan Partisi Tabel**

## **📌 Latar Belakang**
Partisi tabel membagi data menjadi blok yang lebih kecil untuk meningkatkan kecepatan akses data.

## **📌 Problem yang Dihadapi**
- Query lambat saat membaca tabel dengan jutaan baris.
- Pencarian berdasarkan tanggal memakan waktu lama.

## **📌 Solusi dan Implementasi**

### **🔹 Membuat Partisi Berdasarkan Tahun**
```sql
CREATE TABLE transactions (
  id INT NOT NULL AUTO_INCREMENT,
  transaction_date DATE NOT NULL,
  amount DECIMAL(10,2) NOT NULL,
  PRIMARY KEY (id, transaction_date)
) PARTITION BY RANGE (YEAR(transaction_date)) (
  PARTITION p0 VALUES LESS THAN (2020),
  PARTITION p1 VALUES LESS THAN (2021),
  PARTITION p2 VALUES LESS THAN (2022),
  PARTITION p3 VALUES LESS THAN (2023)
);
```

### **📌 Kesimpulan**
Partisi tabel membantu mengurangi beban query dengan hanya membaca data yang relevan, sehingga meningkatkan efisiensi akses data.

---

# **📌 Kesimpulan Akhir**
Dari praktikum yang telah dilakukan, diperoleh beberapa kesimpulan penting:
1. **Konfigurasi MySQL yang optimal** meningkatkan keamanan dan efisiensi database.
2. **Manajemen user dan role** mempermudah pengelolaan hak akses dan keamanan sistem.
3. **Optimasi bottleneck** dengan indeks dan buffer pool size dapat mengurangi waktu eksekusi query.
4. **Partisi tabel** sangat berguna dalam menangani dataset besar agar lebih cepat diakses.

Dengan memahami dan menerapkan teknik-teknik ini, kita dapat mengelola database dengan lebih baik dan optimal! 🚀

---

# **📌 Referensi**
- [MySQL Documentation](https://dev.mysql.com/doc/)
- [SQL Indexing Explained](https://use-the-index-luke.com/)
- [Partitioning in MySQL](https://dev.mysql.com/doc/refman/8.0/en/partitioning-overview.html)

---

✍ **Ditulis oleh:** Faridatul Kasanah
