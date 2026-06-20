# PROGRES 2 – PERANCANGAN BASIS DATA

## Sistem Manajemen Aset dan Barang Inventaris Kantor

---

# 1. ERD (Entity Relationship Diagram)

## Entitas dan Relasi

```text
USERS
│
├── id_user (PK)
├── nama_user
├── username
├── password
└── role

        1
        │
        │
        N

PEMINJAMAN
├── id_peminjaman (PK)
├── id_aset (FK)
├── id_user (FK)
├── tanggal_pinjam
├── tanggal_kembali
└── status_peminjaman

        N
        │
        │
        1

ASET
├── id_aset (PK)
├── kode_aset
├── nama_aset
├── id_kategori (FK)
├── id_lokasi (FK)
├── tanggal_pengadaan
├── nilai_aset
├── kondisi
└── status

        │
   ┌────┴────┐
   │         │
   │         │
   1         1
   │         │
   N         N

KATEGORI     LOKASI
├── id_kategori(PK)   ├── id_lokasi(PK)
├── nama_kategori     ├── nama_lokasi
└── deskripsi         └── keterangan

        1
        │
        │
        N

PERAWATAN
├── id_perawatan (PK)
├── id_aset (FK)
├── tanggal_perawatan
├── jenis_perawatan
├── biaya
└── keterangan
```

---

# 2. Penjelasan Entitas dan Relasi

## 2.1 Entitas Users

Menyimpan data pengguna yang dapat mengakses sistem.

Relasi:

* Satu user dapat melakukan banyak peminjaman.
* Relasi:

```text
Users (1) ----- (N) Peminjaman
```

---

## 2.2 Entitas Kategori

Menyimpan kategori aset.

Contoh:

* Elektronik
* Furniture
* Kendaraan

Relasi:

```text
Kategori (1) ----- (N) Aset
```

Satu kategori dapat memiliki banyak aset.

---

## 2.3 Entitas Lokasi

Menyimpan lokasi penempatan aset.

Contoh:

* Ruang IT
* Ruang Administrasi
* Gudang

Relasi:

```text
Lokasi (1) ----- (N) Aset
```

---

## 2.4 Entitas Aset

Merupakan entitas utama yang menyimpan seluruh data inventaris kantor.

Relasi:

```text
Kategori (1) ---- (N) Aset
Lokasi (1) ---- (N) Aset
Aset (1) ---- (N) Peminjaman
Aset (1) ---- (N) Perawatan
```

---

## 2.5 Entitas Peminjaman

Mencatat proses peminjaman aset oleh pegawai.

Relasi:

```text
Users (1) ---- (N) Peminjaman
Aset (1) ---- (N) Peminjaman
```

---

## 2.6 Entitas Perawatan

Mencatat seluruh aktivitas perawatan aset.

Relasi:

```text
Aset (1) ---- (N) Perawatan
```

---

# 3. Kamus Data (Data Dictionary)

## Tabel Users

| Field     | Tipe Data    | Keterangan     |
| --------- | ------------ | -------------- |
| id_user   | INT          | Primary Key    |
| nama_user | VARCHAR(100) | Nama pengguna  |
| username  | VARCHAR(50)  | Username login |
| password  | VARCHAR(255) | Password       |
| role      | VARCHAR(20)  | Hak akses      |

---

## Tabel Kategori

| Field         | Tipe Data    | Keterangan         |
| ------------- | ------------ | ------------------ |
| id_kategori   | INT          | Primary Key        |
| nama_kategori | VARCHAR(100) | Nama kategori      |
| deskripsi     | TEXT         | Deskripsi kategori |

---

## Tabel Lokasi

| Field       | Tipe Data    | Keterangan        |
| ----------- | ------------ | ----------------- |
| id_lokasi   | INT          | Primary Key       |
| nama_lokasi | VARCHAR(100) | Nama lokasi       |
| keterangan  | TEXT         | Keterangan lokasi |

---

## Tabel Aset

| Field             | Tipe Data     | Keterangan        |
| ----------------- | ------------- | ----------------- |
| id_aset           | INT           | Primary Key       |
| kode_aset         | VARCHAR(20)   | Kode aset         |
| nama_aset         | VARCHAR(100)  | Nama aset         |
| id_kategori       | INT           | Foreign Key       |
| id_lokasi         | INT           | Foreign Key       |
| tanggal_pengadaan | DATE          | Tanggal pengadaan |
| nilai_aset        | DECIMAL(15,2) | Nilai aset        |
| kondisi           | VARCHAR(30)   | Kondisi aset      |
| status            | VARCHAR(30)   | Status aset       |

---

## Tabel Peminjaman

| Field             | Tipe Data   | Keterangan       |
| ----------------- | ----------- | ---------------- |
| id_peminjaman     | INT         | Primary Key      |
| id_aset           | INT         | Foreign Key      |
| id_user           | INT         | Foreign Key      |
| tanggal_pinjam    | DATE        | Tanggal pinjam   |
| tanggal_kembali   | DATE        | Tanggal kembali  |
| status_peminjaman | VARCHAR(20) | Status transaksi |

---

## Tabel Perawatan

| Field             | Tipe Data     | Keterangan        |
| ----------------- | ------------- | ----------------- |
| id_perawatan      | INT           | Primary Key       |
| id_aset           | INT           | Foreign Key       |
| tanggal_perawatan | DATE          | Tanggal perawatan |
| jenis_perawatan   | VARCHAR(100)  | Jenis perawatan   |
| biaya             | DECIMAL(15,2) | Biaya             |
| keterangan        | TEXT          | Catatan           |

---

# 4. Normalisasi

## UNF (Unnormalized Form)

```text
DATA_ASET

id_aset
nama_aset
kategori
lokasi
nama_user
tanggal_pinjam
jenis_perawatan
biaya_perawatan
```

Masih terdapat data berulang dan redundansi.

---

## 1NF (First Normal Form)

Memisahkan data menjadi atribut bernilai tunggal.

```text
ASET
(id_aset, nama_aset, kategori, lokasi)

PEMINJAMAN
(id_peminjaman, nama_user, tanggal_pinjam)

PERAWATAN
(id_perawatan, jenis_perawatan, biaya)
```

---

## 2NF (Second Normal Form)

Menghilangkan ketergantungan parsial.

```text
USERS
KATEGORI
LOKASI
ASET
PEMINJAMAN
PERAWATAN
```

Setiap atribut bergantung penuh pada primary key.

---

## 3NF (Third Normal Form)

Menghilangkan ketergantungan transitif.

Struktur akhir:

```text
USERS
(id_user, nama_user, username, password, role)

KATEGORI
(id_kategori, nama_kategori, deskripsi)

LOKASI
(id_lokasi, nama_lokasi, keterangan)

ASET
(id_aset, kode_aset, nama_aset,
id_kategori, id_lokasi,
tanggal_pengadaan, nilai_aset,
kondisi, status)

PEMINJAMAN
(id_peminjaman, id_aset,
id_user, tanggal_pinjam,
tanggal_kembali, status_peminjaman)

PERAWATAN
(id_perawatan, id_aset,
tanggal_perawatan,
jenis_perawatan, biaya,
keterangan)
```

Database telah memenuhi bentuk normal ketiga (3NF).

---

# 5. Revisi Analisis Kebutuhan

Untuk saat ini ada revisi terhadap Progres 1 karena seluruh kebutuhan fungsional, kebutuhan data, dan diagram proses telah sesuai dengan rancangan basis data.
