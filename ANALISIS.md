# Analisis dan Perbaikan Permasalahan Docker Compose

## Permasalahan 1

### Gejala
`docker compose up` gagal dijalankan dengan error parsing YAML pada file `docker-compose.yml`.

### Penyebab
Kata kunci `services` tidak diikuti tanda titik dua (`:`), sehingga file YAML tidak valid secara sintaks.

### Solusi
Mengubah:

```yaml
services
```

menjadi:

```yaml
services:
```

---

## Permasalahan 2

### Gejala
Container `web3` gagal di-build oleh Docker Compose.

### Penyebab
Build context pada service `web3` mengarah ke direktori `./web33` yang tidak ada. Direktori yang benar adalah `./web3`.

### Solusi
Mengubah:

```yaml
context: ./web33
```

menjadi:

```yaml
context: ./web3
```

---

## Permasalahan 3

### Gejala
Container `web1` gagal terhubung ke database.

### Penyebab
Environment variable `DB_HOST` pada service `web1` diisi dengan nilai `mysql`, padahal nama service database di Docker Compose adalah `db`.

### Solusi
Mengubah:

```yaml
DB_HOST: mysql
```

menjadi:

```yaml
DB_HOST: db
```

---

## Permasalahan 4

### Gejala
Container `web2` gagal terhubung ke database meskipun konfigurasi lainnya benar.

### Penyebab
Environment variable `DB_PASS` pada service `web2` diisi dengan nilai `wrongpassword`, sedangkan password yang terdaftar di database adalah `student123`.

### Solusi
Mengubah:

```yaml
DB_PASS: wrongpassword
```

menjadi:

```yaml
DB_PASS: student123
```

---

## Permasalahan 5

### Gejala
Request dari Nginx tidak bisa diteruskan ke `web3`, sehingga load balancing tidak berjalan sempurna.

### Penyebab
Service `web3` tidak terdaftar pada network `frontend` di `docker-compose.yml`.

### Solusi
Menambahkan network `frontend` pada service `web3`.

Sebelum:

```yaml
networks:
  - backend
```

Sesudah:

```yaml
networks:
  - frontend
  - backend
```

---

## Permasalahan 6

### Gejala
Docker Compose memberikan warning tentang volume yang tidak terdefinisi.

### Penyebab
Service `db` menggunakan volume bernama `db-data`, tetapi pada bagian `volumes` di level root hanya terdefinisi `database-data`.

### Solusi
Menyamakan nama volume menjadi:

```yaml
volumes:
  db-data:
```

---

## Permasalahan 7

### Gejala
Nginx gagal start karena konfigurasi tidak dapat dibaca.

### Penyebab
File `nginx/nginx.conf` mengandung markdown code fences (` ```nginx ` dan ` ``` `) yang tidak valid sebagai sintaks konfigurasi Nginx.

### Solusi
Menghapus baris:

```text
```nginx
```

dan

```text
```
```

sehingga hanya tersisa konfigurasi Nginx yang valid.

---

## Permasalahan 8

### Gejala
Nginx tidak dapat meneruskan request ke `web1`.

### Penyebab
Pada `nginx/nginx.conf`, upstream backend mendefinisikan:

```nginx
server web11:80;
```

Padahal nama host yang benar adalah `web1`.

### Solusi
Mengubah menjadi:

```nginx
server web1:80;
```

---

## Permasalahan 9

### Gejala
Nginx gagal terhubung ke `web3` saat melakukan load balancing.

### Penyebab
Pada konfigurasi upstream digunakan:

```nginx
server web3:8080;
```

Padahal Apache di dalam container berjalan pada port `80`.

### Solusi
Mengubah menjadi:

```nginx
server web3:80;
```

---

## Permasalahan 10

### Gejala
Container `web1` gagal di-build.

### Penyebab
File `web1/Dockerfile` menggunakan base image yang salah:

```dockerfile
FROM php:8.2-apach
```

### Solusi
Mengubah menjadi:

```dockerfile
FROM php:8.2-apache
```

---

## Permasalahan 11

### Gejala
Container `web3` gagal di-build.

### Penyebab
File `web3/Dockerfile` menggunakan base image yang salah:

```dockerfile
FROM php:8.2-apche
```

### Solusi
Mengubah menjadi:

```dockerfile
FROM php:8.2-apache
```

---

## Permasalahan 12

### Gejala
Halaman `web2` menampilkan label container yang salah.

### Penyebab
Pada file `web2/index.php`, label container ditulis:

```php
WEB-WEB
```

### Solusi
Mengubah menjadi:

```php
WEB-2
```

---

## Permasalahan 13

### Gejala
Halaman `web3` menampilkan label container yang salah.

### Penyebab
Pada file `web3/index.php`, label container ditulis:

```php
WEB-WOB
```

### Solusi
Mengubah menjadi:

```php
WEB-3
```

---

## Permasalahan 14

### Gejala
File `db/init.sql` gagal dieksekusi saat container database pertama kali dijalankan.

### Penyebab
File `init.sql` mengandung markdown code fences (` ```sql ` dan ` ``` `) yang tidak valid sebagai sintaks SQL.

### Solusi
Menghapus baris:

```text
```sql
```

dan

```text
```
```

sehingga hanya tersisa query SQL yang valid.
