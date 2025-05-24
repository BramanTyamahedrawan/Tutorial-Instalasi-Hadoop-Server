# **Clone VM dan Konfigurasi Hosts**

# ## **Langkah 1: Clone VM**

1. Buka VirtualBox.
2. Matikan VM "Hadoop-Template" jika masih berjalan.
3. Pilih VM "Hadoop-Template" yang telah Anda buat sebelumnya.
4. Klik kanan pada VM tersebut dan pilih "Clone" atau "Klon".
5. Beri nama VM baru sesuai perannya dalam cluster Hadoop. Disini kita akan membuat beberapa VM untuk 6 cluster Hadoop.
   - **hadoop-primary** (Master Node)
   - **hadoop-secondary-1** (Slave Node 1)
   - **hadoop-secondary-2** (Slave Node 2)
   - **zookeeper-1** (Zookeeper Node 1)
   - **zookeeper-2** (Zookeeper Node 2)
   - **zookeeper-3** (Zookeeper Node 3)
6. Pilih opsi "Full Clone" untuk membuat salinan lengkap dari VM.
7. Klik "Clone" untuk memulai proses kloning. Tunggu hingga proses selesai.
8. Setelah selesai, Anda akan melihat VM baru di daftar VirtualBox.
9. Ulangi langkah 3-8 untuk membuat semua VM yang diperlukan.

## **Langkah 2: Konfigurasi Hostname**

1. Setelah clone VM selesai, hidupkan setiap VM satu per satu.
2. Login ke setiap VM menggunakan username dan password yang telah Anda buat sebelumnya.
3. Ubah hostname untuk setiap VM sesuai dengan nama yang telah Anda tentukan:

   **(Hostname adalah nama unik yang digunakan untuk mengidentifikasi setiap VM dalam jaringan. Pastikan untuk menggunakan nama yang sesuai dengan peran masing-masing VM dalam cluster Hadoop. Untuk penamaannya tidak ada aturan khusus, namun disarankan untuk menggunakan nama yang mudah diingat dan sesuai dengan fungsinya dalam cluster.)**

   - Untuk **hadoop-primary**:
     ```bash
     sudo hostnamectl set-hostname hadoop-primary
     ```
   - Untuk **hadoop-secondary-1**:
     ```bash
     sudo hostnamectl set-hostname hadoop-secondary-1
     ```
   - Untuk **hadoop-secondary-2**:
     ```bash
     sudo hostnamectl set-hostname hadoop-secondary-2
     ```
   - Untuk **zookeeper-1**:
     ```bash
     sudo hostnamectl set-hostname zookeeper-1
     ```
   - Untuk **zookeeper-2**:
     ```bash
     sudo hostnamectl set-hostname zookeeper-2
     ```
   - Untuk **zookeeper-3**:
     ```bash
     sudo hostnamectl set-hostname zookeeper-3
     ```

4. Setelah mengubah hostname, reboot setiap VM untuk menerapkan perubahan:
   ```bash
   sudo reboot
   ```
5. Setelah reboot, pastikan hostname telah berubah dengan menjalankan perintah:
   ```bash
    hostname
   ```

## **Langkah 3: Konfigurasi Hosts File**

1. Buka file `/etc/hosts` pada setiap VM menggunakan editor teks seperti `nano` atau `vim`:
   ```bash
   sudo nano /etc/hosts
   ```
2. Tambahkan entri berikut di akhir file untuk menghubungkan hostname dengan alamat IP masing-masing VM. Sesuaikan alamat IP sesuai dengan konfigurasi jaringan yang Anda buat sebelumnya:

   ```yaml
   #hadoop user and zookeeper node
   192.168.50.10 hadoop-primary
   192.168.50.20 hadoop-secondary-1
   192.168.50.30 hadoop-secondary-2
   192.168.50.40 zookeeper-1
   192.168.50.50 zookeeper-2
   192.168.50.60 zookeeper-3
   ```

   **bila ada IP seperti ini**

   ```yaml
   127.0.1.1 hadoop-primary (hapus saja baris ini)
   ```

   **HAPUS saja baris tersebut, karena itu adalah IP lokal yang tidak diperlukan dalam konfigurasi ini.**

3. Simpan perubahan dengan menekan `CTRL + X`, lalu tekan `Y` untuk mengonfirmasi penyimpanan, dan tekan `Enter` untuk keluar dari editor.
4. Ulangi langkah 1-3 untuk setiap VM yang telah Anda buat
5. Setelah lakukan reboot pada setipa VM

   ```bash
   sudo reboot
   ```

6. Setelah reboot, pastikan konfigurasi hosts telah berhasil dengan menjalankan perintah berikut pada setiap VM:
   ```bash
   ping hadoop-primary
   ping hadoop-secondary-1
   ping hadoop-secondary-2
   ping zookeeper-1
   ping zookeeper-2
   ping zookeeper-3
   ```
   pastikan setiap VM dapat saling ping satu sama lain menggunakan hostname yang telah Anda atur.
