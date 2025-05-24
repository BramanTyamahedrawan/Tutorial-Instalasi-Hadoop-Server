# ** Download Linux dan Virtual Machine (VM)**

## **Langkah 1: Download Virtual Box**

1. Buka situs resmi VirtualBox di [https://www.virtualbox.org/](https://www.virtualbox.org/).
2. Klik pada tombol "Download VirtualBox" yang sesuai dengan sistem operasi Anda (Windows, macOS, Linux, dll.).
3. Setelah selesai mengunduh, buka file installer dan ikuti petunjuk untuk menginstal VirtualBox di komputer Anda.
4. Setelah instalasi selesai, buka VirtualBox.
5. Lihat pojok kiri atas, ada tab "File", klik tab tersebut.
6. Pilih "Tools", kemudian pilih "Network".
7. Di tab "Host-only Networks", klik ikon "Create" (ikon dengan tanda plus).
8. Setelah itu, akan muncul "VirtualBox Host-Only Ethernet Adapter" di daftar.
9. Ganti IPv4 Address sesuaikan dengan keinginan anda, saya pakai "192.168.50.1/24"
10. Ganti IPv4 Network Mask sesuaikan dengan keinginan anda, saya pakai "255.255.255.0"
11. Klik "Apply" untuk menyimpan pengaturan.

## **Langkah 2: Download ISO Linux**

1. Pilih distribusi Linux yang ingin Anda instal. Beberapa distribusi populer adalah **(Harus Server)**:
   - Ubuntu: [https://ubuntu.com/download](https://ubuntu.com/download)
   - Fedora: [https://getfedora.org/](https://getfedora.org/)
   - Debian: [https://www.debian.org/distrib/](https://www.debian.org/distrib/)
   - CentOS: [https://www.centos.org/download/](https://www.centos.org/download/)
2. Klik pada tautan unduhan untuk versi yang Anda pilih.
3. Pilih versi ISO yang sesuai dengan arsitektur komputer Anda (32-bit atau 64-bit).
4. Setelah selesai mengunduh, simpan file ISO di lokasi yang mudah diakses di komputer Anda.

## **Langkah 3: Buat Mesin Virtual di VirtualBox**

1. Buka VirtualBox.
2. Klik pada tombol "New" atau "Buat" untuk membuat mesin virtual baru.
3. Isi informasi Dasar VM:
   - **Nama**: Hadoop-Template
   - **Folder Mesin**: Biarkan default atau pilih lokasi penyimpanan VM.
   - **Tipe**: Linux
   - **Versi**: Ubuntu (64-bit) atau sesuai dengan distribusi yang Anda pilih.
   - Klik "Next".
4. Atur Memori:
   - Pilih jumlah RAM yang ingin Anda alokasikan untuk mesin virtual. Disarankan minimal 2048 MB (2 GB) untuk distribusi modern.
   - Klik "Next".
5. Buat Hard Disk Virtual:
   - Pilih "Create a virtual hard disk now" dan klik "Create".
6. Pilih Tipe Hard Disk:
   - Pilih "VDI (VirtualBox Disk Image)" dan klik "Next".
7. Atur Penyimpanan Hard Disk:
   - Pilih "Dynamically allocated" untuk menghemat ruang disk.
   - Klik "Next".
8. Atur Ukuran Hard Disk:
   - Tentukan ukuran hard disk virtual. Disarankan minimal 20 GB.
   - Klik "Create".
9. Setelah selesai, Anda akan melihat mesin virtual baru di daftar VirtualBox.

## **Langkah 4: Konfigurasi Mesin Virtual**

1. Pilih VM "Hadoop-Template" yang baru dibuat di daftar VirtualBox.
2. Klik pada tombol "Settings" atau "Pengaturan".
3. Di tab "System":
   - Pastikan opsi "Enable EFI (special OSes only)" tidak dicentang.
   - Di tab "Processor", alokasikan jumlah CPU yang sesuai (disarankan minimal 2 CPU).
   - Di tab boot, pastikan "Optical" berada di atas "Hard Disk".
4. Di tab "Display":
   - Atur "Video Memory" ke 16 MB atau lebih.
5. Di tab "Storage":
   - Klik pada ikon CD atau "Empty" di bawah "Controller: IDE".
   - Klik pada ikon disk di sebelah kanan dan pilih "Choose a disk file...".
   - Pilih file ISO Ubuntu Server yang telah Anda unduh.
   - Klik "OK" untuk menyimpan pengaturan.
6. Di tab "Network":
   - Tab "Adapter 1"
     - Pastikan "Enable Network Adapter" dicentang.
     - Pilih "NAT" sebagai koneksi jaringan.
   - Tab "Adapter 2"
     - Centang "Enable Network Adapter".
     - Pilih "Internal Network" sebagai koneksi jaringan.
     - VirtualBox Host-Only Ethernet Adapter (pilih yang sesuai dengan yang telah Anda buat sebelumnya).
7. Klik "OK" untuk menyimpan semua pengaturan.
