# gRPC Polyglot Lab: Go, Python, dan Node.js

## 1. Deskripsi Proyek

Proyek ini merupakan implementasi komunikasi gRPC Polyglot yang menggunakan tiga bahasa pemrograman, yaitu Go sebagai server, Python sebagai client pertama, dan Node.js sebagai client kedua.

Komunikasi menggunakan gRPC dan Protocol Buffers untuk mendefinisikan struktur data serta layanan. Server menyediakan layanan `GetUserProfile` untuk mengambil data pengguna berdasarkan ID.

## 2. Tujuan

1. Memahami konsep komunikasi menggunakan gRPC.
2. Membuat definisi layanan menggunakan Protocol Buffers.
3. Membuat gRPC Server menggunakan Go.
4. Membuat client gRPC menggunakan Python dan Node.js.
5. Menguji komunikasi antara server dan client yang berbeda bahasa pemrograman.
6. Mengelola kode sumber menggunakan Git dan GitHub.

## 3. Teknologi yang Digunakan

- Visual Studio Code
- Go
- Python
- Node.js dan npm
- Protocol Buffers Compiler (`protoc`)
- gRPC
- Git dan GitHub

## 4. Struktur Proyek

```text
grpc-polyglot-lab/
├── .gitignore
├── README.md
├── user.proto
├── descriptor.pb
├── go-server/
│   ├── main.go
│   ├── go.mod
│   ├── go.sum
│   └── pb/
│       ├── user.pb.go
│       └── user_grpc.pb.go
├── python-client/
│   ├── client.py
│   ├── user_pb2.py
│   ├── user_pb2_grpc.py
│   └── .venv/
└── node-client/
    ├── client.js
    ├── package.json
    ├── package-lock.json
    └── node_modules/
```

Folder `.venv` dan `node_modules` merupakan folder instalasi lokal dan tidak perlu diunggah ke GitHub.

## 5. Langkah-Langkah Pengerjaan

### Langkah 1. Persiapan Lingkungan

Instal dan siapkan Git, Go, Python, Node.js, npm, serta Protocol Buffers Compiler (`protoc`).

Periksa instalasi melalui terminal:

```powershell
git --version
go version
python --version
node --version
npm --version
protoc --version
```

Pastikan setiap perintah menampilkan versi perangkat lunak yang terpasang.

### Langkah 2. Membuat Folder Proyek

Buat folder utama bernama `grpc-polyglot-lab`, kemudian buka folder tersebut melalui Visual Studio Code.

Siapkan folder `go-server`, `python-client`, dan `node-client` untuk memisahkan implementasi server dan masing-masing client.

### Langkah 3. Membuat File Protocol Buffers

Buat file `user.proto` pada folder utama proyek.

Definisikan pesan `UserRequest` yang memiliki atribut `user_id`, pesan `UserResponse` yang memiliki atribut `name` dan `email`, serta layanan `UserService` dengan prosedur `GetUserProfile`.

File ini menjadi acuan struktur komunikasi antara server dan client.

### Langkah 4. Menghasilkan Descriptor Protokol

Jalankan perintah berikut dari folder utama:

```powershell
protoc --proto_path=. --descriptor_set_out=descriptor.pb user.proto
```

Perintah tersebut menghasilkan file `descriptor.pb` berdasarkan definisi pada `user.proto`.

### Langkah 5. Membuat gRPC Server dengan Go

Masuk ke folder server:

```powershell
cd go-server
go mod init go-server
go get google.golang.org/grpc
```

Hasilkan kode Go dari definisi protokol:

```powershell
protoc --proto_path=../ --go_out=. --go-grpc_out=. ../user.proto
```

Buat file `main.go` untuk mengimplementasikan layanan `GetUserProfile`. Server menerima permintaan ID pengguna dan mengembalikan data yang sesuai.

Jalankan server:

```powershell
go mod tidy
go run .
```

Server berjalan pada port `50051`. Biarkan terminal server tetap terbuka selama pengujian client.

### Langkah 6. Membuat Client Python

Buka terminal baru, lalu masuk ke folder `python-client`.

Buat dan aktifkan lingkungan virtual:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Instal library yang diperlukan:

```powershell
python -m pip install grpcio grpcio-tools
```

Hasilkan kode Python dari file protokol:

```powershell
python -m grpc_tools.protoc --proto_path=".." --python_out="." --grpc_python_out="." "..\user.proto"
```

Buat file `client.py` untuk menghubungkan client ke `localhost:50051` dan memanggil layanan `GetUserProfile`.

Jalankan client:

```powershell
python client.py
```

Jika berhasil, client menampilkan nama dan email pengguna yang dikirim oleh server.

### Langkah 7. Membuat Client Node.js

Buka terminal baru dan masuk ke folder `node-client`.

Inisialisasi proyek Node.js:

```powershell
npm init -y
```

Instal library gRPC:

```powershell
npm install @grpc/grpc-js @grpc/proto-loader
```

Buat file `client.js` untuk membaca `user.proto`, membangun koneksi ke `localhost:50051`, dan memanggil layanan `GetUserProfile`.

Jalankan client:

```powershell
node client.js
```

Jika berhasil, client menampilkan data pengguna yang diterima dari Go Server.

### Langkah 8. Menguji Komunikasi gRPC

Pastikan Go Server masih berjalan pada port `50051`.

Jalankan Client Python dengan ID pengguna `1`, kemudian jalankan Client Node.js dengan ID yang sama.

Hasil pengujian yang diperoleh:

```text
=== Hasil Client Python ===
Nama  : Budi Santoso
Email : budi@universitas.ac.id
```

```text
=== Hasil Client Node.js ===
Nama  : Budi Santoso
Email : budi@universitas.ac.id
```

Selanjutnya, ubah ID pengguna pada Client Node.js menjadi `2` untuk menguji respons ketika pengguna tidak ditemukan.

Hasil pengujian:

```text
=== Hasil Client Node.js ===
Nama  : User Tidak Ditemukan
Email : -
```

Setelah pengujian selesai, kembalikan ID pengguna menjadi `1`.

### Langkah 9. Menyimpan Proyek Menggunakan Git

Buka terminal di folder utama proyek.

Periksa status repository:

```powershell
git status
```

Tambahkan file proyek dan buat commit:

```powershell
git add .
git commit -m "Implement Go gRPC server and polyglot clients"
```

Periksa kembali status Git untuk memastikan perubahan sudah tersimpan.

### Langkah 10. Mengunggah Proyek ke GitHub

Buat repository GitHub kosong dengan nama `grpc-polyglot-lab`.

Hubungkan repository lokal dengan repository GitHub:

```powershell
git remote add origin https://github.com/USERNAME/grpc-polyglot-lab.git
```

Ganti `USERNAME` dengan nama akun GitHub pemilik repository.

Unggah branch `master`:

```powershell
git push -u origin master
```

Pastikan folder dan file proyek terlihat pada halaman repository GitHub.

## 6. Hasil Akhir

Berdasarkan pengujian, Go Server berhasil menerima permintaan melalui gRPC dan memberikan respons kepada Client Python serta Client Node.js.

Client dengan ID pengguna `1` menerima data Budi Santoso, sedangkan ID pengguna `2` menerima respons `User Tidak Ditemukan`.

Kode sumber proyek juga berhasil disimpan menggunakan Git dan diunggah ke GitHub.

## 7. Repository GitHub

Repository proyek dapat diakses melalui tautan berikut:

https://github.com/syifaasyafitri087-gif/grpc-polyglot-lab

## 8. Kesimpulan

Implementasi gRPC Polyglot berhasil dilakukan menggunakan Go sebagai server serta Python dan Node.js sebagai client. Protocol Buffers digunakan untuk menyamakan definisi pesan dan layanan, sedangkan Git dan GitHub digunakan untuk mengelola serta menyimpan kode sumber proyek.

Praktikum ini memberikan pemahaman mengenai komunikasi antarlayanan dengan bahasa pemrograman berbeda menggunakan gRPC.