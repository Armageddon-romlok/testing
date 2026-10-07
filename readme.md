# Minpro-3-PBO-SistemRentalSkateboard

# 1. Deskripsi Singkat Program
Program ini adalah aplikasi berbasis *Command Line Interface* (CLI) untuk mengelola data transaksi **Sistem Manajemen Rental Skateboard**. Program ini memungkinkan admin untuk melakukan operasi CRUD (Create, Read, Update, Delete) pada data penyewaan. Sistem ini mendukung dua kategori penyewaan papan, yaitu *Street Skate* dan *Cruiser Skate*, dengan kalkulasi biaya sewa yang dinamis berdasarkan durasi hari dan tarif papan yang dipilih.

<img width="385" height="201" alt="{07A42B13-E95D-4E6E-8E27-F22E8540707B}" src="https://github.com/user-attachments/assets/b3ec4d8f-98a5-4251-8b9d-e2b62ac01bf1" />


# 2. Penjelasan Struktur Package
Proyek ini dirombak secara penuh dan dibangun menggunakan arsitektur **MVC (Model-View-Controller)** untuk memisahkan antara antarmuka, logika bisnis, dan struktur data:
**`model`**: Package ini berisi cetak biru (blueprint) data dan aturan bisnis murni. Berisi kelas `Skateboard`, `StreetSkate`, `CruiserSkate`, `Penyewa`, dan antarmuka `LayananRental`.
**`view`**: Package ini berisi kelas `RentalView` yang bertugas khusus menangani I/O pengguna, menampilkan menu CLI, membaca input dengan `Scanner`, serta menangani blok *try-catch* (Error Handling).
**`controller`**: Package ini berisi kelas `RentalController` yang bertindak sebagai "otak" sistem. Mengelola `ArrayList<Penyewa>` dan menangani logika operasi CRUD (Tambah, Tampil, Update, Hapus) tanpa ada campur tangan sintaks *user interface* (System.out.print).
**`main`**: Package ini berisi `MainApp` yang berfungsi secara eksklusif sebagai *entry point* untuk menjalankan program.

<img width="320" height="317" alt="{2C9643D7-FB00-431D-89C2-0107EDC0B258}" src="https://github.com/user-attachments/assets/eacaedb4-5b06-4615-b60f-bf735ff7f531" />


# 3. Penjelasan Alur Program
1. Saat program dijalankan melalui `MainApp`, sistem akan memanggil metode dari `RentalView` untuk menampilkan Menu Utama (1-5).
2. Sistem akan meload *dummy data* awal melalui konstruktor `RentalController` untuk keperluan pengujian.
3. Jika pengguna memilih menu **Tambah Data (1)**, `View` akan meminta input data, lalu mengirimkan data tersebut ke `Controller` untuk disimpan ke dalam memori `ArrayList` di dalam `Model`.

<img width="739" height="414" alt="{B589CD10-5AD8-4687-9E06-CA67FDB022FC}" src="https://github.com/user-attachments/assets/edae733d-5596-40c6-a0cb-dc860f369cd8" />


4. Jika pengguna memilih menu **Tampil Data (2)**, `View` akan meminta koleksi data dari `Controller` lalu mencetak detailnya ke layar secara dinamis (*Dynamic Method Dispatch*).

<img width="485" height="646" alt="{73C5ABA0-FBBA-4A72-8C46-093042D9B40E}" src="https://github.com/user-attachments/assets/45137e6a-8bdb-4694-8f7f-ebef00d66694" />
<img width="503" height="293" alt="{491CBEC5-195E-4CCA-8155-54482D77FAA1}" src="https://github.com/user-attachments/assets/6a3d4a05-e8e8-4a0f-bfed-6e90574c88c6" />


5. Pada menu **Update (3)** dan **Delete (4)**, `View` meminta ID Rental yang ingin diproses, mendelegasikannya ke `Controller` untuk dicari, dan jika ditemukan, `Controller` akan mengubah/menghapus objek tersebut dari memori.

<img width="486" height="92" alt="{1A3BFEC2-CFB9-4B77-835C-6014E3E5917A}" src="https://github.com/user-attachments/assets/c633f828-b1c8-459c-a2cc-e8c1a6596e88" />
<img width="482" height="70" alt="{DC258FF5-3D9F-49EA-978C-15FF3E929997}" src="https://github.com/user-attachments/assets/15e5ab44-1113-432c-8dc3-bbb29a592593" />


6. Program akan terus berulang (loop) selama pengguna tidak menekan opsi **Keluar (5)**. Setiap input karakter salah dilindungi oleh struktur *Exception Handling*.

# 4. Penjelasan Penerapan Encapsulation dan Inheritance
*   **Encapsulation (Pengkapsulan):**
    *   Penerapan *Access Modifier* `private` pada atribut spesifik seperti `ukuranRoda` pada `StreetSkate`, `panjangPapan` pada `CruiserSkate`, dan `ArrayList` pada kelas Controller.
    *   Penggunaan **Setter** (seperti `setLamaSewa` di kelas `Penyewa`) untuk memvalidasi masukan data (jika lama sewa diinput 0 atau negatif, setter memaksa nilainya menjadi minimal 1 hari).
    *   Penggunaan *keyword* `final` pada atribut `idPapan` dan `idRental` agar nilainya bersifat mutlak (konstan) dan tidak bisa dimodifikasi setelah objek dibuat.


*   **Inheritance (Pewarisan):** 
    *   Terdapat penggunaan *keyword* `extends`. Kelas `StreetSkate` dan `CruiserSkate` merupakan kelas turunan (subclass) yang mewarisi atribut (`idPapan`, `merk`, `tarifSewa`) dan method dari kelas induk (superclass) `Skateboard`.

# 5. Penjelasan Penerapan Polymorphism dan Abstraction
*   **Abstraction (Abstraksi):**
    *   **Abstract Class:** Kelas `Skateboard` dideklarasikan menggunakan *keyword* `abstract`. Hal ini menjadikan kelas tersebut sebagai *blueprint* murni yang tidak bisa diinstansiasi secara langsung.
    *   **Abstract Method:** Di dalam kelas `Skateboard` terdapat `public abstract void tampilkanDetailPapan();` yang hanya berisi deklarasi metode tanpa tubuh *(body)*. Metode ini memaksa (*enforce*) setiap subclass-nya untuk memiliki format tampilannya masing-masing.


*   **Polymorphism (Polimorfisme):**
    *   **Overriding:** Kelas `StreetSkate` dan `CruiserSkate` menimpa metode dari kelas induknya dengan menggunakan anotasi `@Override` pada metode `tampilkanDetailPapan()` untuk mencetak spesifikasi yang berbeda (satu menampilkan ukuran roda, satu menampilkan panjang papan).
    *   **Overloading:** Pada kelas `Penyewa` terdapat dua metode bernama sama yaitu `hitungTotalBiaya()`. Metode pertama berdiri tanpa parameter (menghitung tarif normal), dan metode kedua menggunakan parameter `double diskon` (menghitung tarif setelah dikenakan potongan/diskon).

# 6. Penjelasan Letak Penerapan Nilai Tambah (Nilai Plus)
Proyek ini mengimplementasikan dua nilai tambah (Fitur Ekstra) di luar persyaratan dasar:
1.  **Penerapan Interface (Antarmuka Kontrak Murni):**
    *   Terdapat pembuatan file `LayananRental.java` di dalam package `model` yang dideklarasikan sebagai `interface`.
    *   Interface ini diimplementasikan (menggunakan *keyword* `implements`) oleh kelas `Penyewa`. Kelas ini mematuhi kontrak dengan mendefinisikan secara paksa *(override)* logika untuk metode `konfirmasiPenyewaan()` dan `cetakStruk()`.
2.  **Validasi Input dan Advanced Error Handling:**
    *   Implementasi struktur `try-catch (InputMismatchException e)` untuk memblokir program *crash* jika pengguna mengetikkan huruf saat sistem meminta input angka.
    *   Terdapat **Helper Method** khusus `bacaInputString()` di dalam kelas View yang berfungsi untuk menolak input kosong *(Anti-Bypass Enter/Spasi kosong)* sehingga integritas data string tetap terjaga.
    *   Validasi logika bisnis di `Controller` (method `isIdAda()`) yang menolak penambahan data jika **ID Rental duplikat/sudah ada** di dalam sistem.

<img width="365" height="136" alt="{087B088D-21DC-41C7-93D5-C99AB1805772}" src="https://github.com/user-attachments/assets/cfae2eb1-71bf-4cbe-9c55-91a2d031b7fe" />
<img width="440" height="196" alt="{0A46F6C9-9EA7-47D0-B9BF-10FBF64CA8D2}" src="https://github.com/user-attachments/assets/09a3f6b4-79ea-4aad-9769-77e76a2c7afd" />

