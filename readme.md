# Minpro-3-PBO-SistemRentalSkateboard

## 1. Deskripsi Singkat Program
Program ini adalah aplikasi berbasis *Command Line Interface* (CLI) untuk mengelola data transaksi **Sistem Manajemen Rental Skateboard**. Program ini memungkinkan admin untuk melakukan operasi CRUD (Create, Read, Update, Delete) pada data penyewaan. Sistem ini mendukung dua kategori penyewaan papan, yaitu *Street Skate* dan *Cruiser Skate*, dengan kalkulasi biaya sewa yang dinamis berdasarkan durasi hari dan tarif papan yang dipilih.

> [TARUH SS DI SINI: Screenshot tampilan awal saat program di-run (Tampilan Menu Utama 1-5)]

## 2. Penjelasan Struktur Package
Proyek ini dirombak secara penuh dan dibangun menggunakan arsitektur **MVC (Model-View-Controller)** untuk memisahkan antara antarmuka, logika bisnis, dan struktur data:
*   **`model`**: Package ini berisi cetak biru (blueprint) data dan aturan bisnis murni. Berisi kelas `Skateboard`, `StreetSkate`, `CruiserSkate`, `Penyewa`, dan antarmuka `LayananRental`.
*   **`view`**: Package ini berisi kelas `RentalView` yang bertugas khusus menangani I/O pengguna, menampilkan menu CLI, membaca input dengan `Scanner`, serta menangani blok *try-catch* (Error Handling).
*   **`controller`**: Package ini berisi kelas `RentalController` yang bertindak sebagai "otak" sistem. Mengelola `ArrayList<Penyewa>` dan menangani logika operasi CRUD (Tambah, Tampil, Update, Hapus) tanpa ada campur tangan sintaks *user interface* (System.out.print).
*   **`main`**: Package ini berisi `MainApp` yang berfungsi secara eksklusif sebagai *entry point* untuk menjalankan program.

> [TARUH SS DI SINI: Screenshot folder struktur project di NetBeans yang memperlihatkan package controller, main, model, dan view]

## 3. Penjelasan Alur Program
1. Saat program dijalankan melalui `MainApp`, sistem akan memanggil metode dari `RentalView` untuk menampilkan Menu Utama (1-5).
2. Sistem akan meload *dummy data* awal melalui konstruktor `RentalController` untuk keperluan pengujian.
3. Jika pengguna memilih menu **Tambah Data (1)**, `View` akan meminta input data, lalu mengirimkan data tersebut ke `Controller` untuk disimpan ke dalam memori `ArrayList` di dalam `Model`.

> [TARUH SS DI SINI: Screenshot saat kamu berhasil melakukan input Tambah Data]

4. Jika pengguna memilih menu **Tampil Data (2)**, `View` akan meminta koleksi data dari `Controller` lalu mencetak detailnya ke layar secara dinamis (*Dynamic Method Dispatch*).

> [TARUH SS DI SINI: Screenshot saat kamu memilih menu Tampil Data dan struk transaksinya muncul]

5. Pada menu **Update (3)** dan **Delete (4)**, `View` meminta ID Rental yang ingin diproses, mendelegasikannya ke `Controller` untuk dicari, dan jika ditemukan, `Controller` akan mengubah/menghapus objek tersebut dari memori.

> [TARUH SS DI SINI: Screenshot saat kamu berhasil melakukan Update lama sewa ATAU Delete data]

6. Program akan terus berulang (loop) selama pengguna tidak menekan opsi **Keluar (5)**. Setiap input karakter salah dilindungi oleh struktur *Exception Handling*.

## 4. Penjelasan Penerapan Encapsulation dan Inheritance
*   **Encapsulation (Pengkapsulan):**
    *   Penerapan *Access Modifier* `private` pada atribut spesifik seperti `ukuranRoda` pada `StreetSkate`, `panjangPapan` pada `CruiserSkate`, dan `ArrayList` pada kelas Controller.
    *   Penggunaan **Setter** (seperti `setLamaSewa` di kelas `Penyewa`) untuk memvalidasi masukan data (jika lama sewa diinput 0 atau negatif, setter memaksa nilainya menjadi minimal 1 hari).
    *   Penggunaan *keyword* `final` pada atribut `idPapan` dan `idRental` agar nilainya bersifat mutlak (konstan) dan tidak bisa dimodifikasi setelah objek dibuat.

> [TARUH SS DI SINI: (Opsional) Screenshot potongan kode yang menunjukkan keyword 'private' atau 'final']

*   **Inheritance (Pewarisan):** 
    *   Terdapat penggunaan *keyword* `extends`. Kelas `StreetSkate` dan `CruiserSkate` merupakan kelas turunan (subclass) yang mewarisi atribut (`idPapan`, `merk`, `tarifSewa`) dan method dari kelas induk (superclass) `Skateboard`.

## 5. Penjelasan Penerapan Polymorphism dan Abstraction
*   **Abstraction (Abstraksi):**
    *   **Abstract Class:** Kelas `Skateboard` dideklarasikan menggunakan *keyword* `abstract`. Hal ini menjadikan kelas tersebut sebagai *blueprint* murni yang tidak bisa diinstansiasi secara langsung.
    *   **Abstract Method:** Di dalam kelas `Skateboard` terdapat `public abstract void tampilkanDetailPapan();` yang hanya berisi deklarasi metode tanpa tubuh *(body)*. Metode ini memaksa (*enforce*) setiap subclass-nya untuk memiliki format tampilannya masing-masing.

> [TARUH SS DI SINI: (Opsional) Screenshot potongan kode `public abstract class Skateboard`]

*   **Polymorphism (Polimorfisme):**
    *   **Overriding:** Kelas `StreetSkate` dan `CruiserSkate` menimpa metode dari kelas induknya dengan menggunakan anotasi `@Override` pada metode `tampilkanDetailPapan()` untuk mencetak spesifikasi yang berbeda (satu menampilkan ukuran roda, satu menampilkan panjang papan).
    *   **Overloading:** Pada kelas `Penyewa` terdapat dua metode bernama sama yaitu `hitungTotalBiaya()`. Metode pertama berdiri tanpa parameter (menghitung tarif normal), dan metode kedua menggunakan parameter `double diskon` (menghitung tarif setelah dikenakan potongan/diskon).

## 6. Penjelasan Letak Penerapan Nilai Tambah (Nilai Plus)
Proyek ini mengimplementasikan dua nilai tambah (Fitur Ekstra) di luar persyaratan dasar:
1.  **Penerapan Interface (Antarmuka Kontrak Murni):**
    *   Terdapat pembuatan file `LayananRental.java` di dalam package `model` yang dideklarasikan sebagai `interface`.
    *   Interface ini diimplementasikan (menggunakan *keyword* `implements`) oleh kelas `Penyewa`. Kelas ini mematuhi kontrak dengan mendefinisikan secara paksa *(override)* logika untuk metode `konfirmasiPenyewaan()` dan `cetakStruk()`.
2.  **Validasi Input dan Advanced Error Handling:**
    *   Implementasi struktur `try-catch (InputMismatchException e)` untuk memblokir program *crash* jika pengguna mengetikkan huruf saat sistem meminta input angka.
    *   Terdapat **Helper Method** khusus `bacaInputString()` di dalam kelas View yang berfungsi untuk menolak input kosong *(Anti-Bypass Enter/Spasi kosong)* sehingga integritas data string tetap terjaga.
    *   Validasi logika bisnis di `Controller` (method `isIdAda()`) yang menolak penambahan data jika **ID Rental duplikat/sudah ada** di dalam sistem.

> [TARUH SS DI SINI: Screenshot saat kamu sengaja input huruf/typo di menu, ATAU input ID yang duplikat, untuk membuktikan error handling-nya berjalan (tidak crash)]
