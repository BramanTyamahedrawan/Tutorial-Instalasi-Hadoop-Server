# **Konfigurasi Network Linux**

## **Langkah 1: Update Sistem**

1. Login ke server Linux Anda.
2. Masukkan Username dan Password yang telah Anda buat saat instalasi.
3. Jalankan perintah berikut untuk memperbarui sistem:
   ```bash
   sudo apt update
   ```
4. Tunggu hingga proses pembaruan selesai.
5. Setelah selesai, jalankan perintah berikut untuk meng-upgrade paket yang sudah terinstal:

   ```bash
   sudo apt upgrade -y
   ```

6. Setelah proses upgrade selesai, reboot sistem untuk memastikan semua perubahan diterapkan:

   ```bash
   sudo reboot
   ```

7. Tunggu beberapa saat hingga sistem reboot. Setelah reboot, Anda akan kembali ke prompt login.

## **Langkah 2: Install Paket yang Diperlukan**

1. Setelah sistem reboot, login kembali ke server.
2. Jalankan perintah berikut untuk menginstal paket yang diperlukan untuk konfigurasi jaringan:

   ```bash
   sudo apt install -y vim net-tools openssh-server wget curl
   ```

3. Tunggu hingga proses instalasi selesai.

## **Langkah 3: Konfigurasi Jaringan IP Statis untuk adapter 2 (Host-Only)**

1. Buka file konfigurasi jaringan dengan editor teks seperti `vim` atau `nano`. Di sini kita akan menggunakan `nano`:

   ```bash
    sudo nano /etc/netplan/00-installer-config.yaml
   ```

2. Disini kita akan menggunakan IP Statis untuk adapter kedua (enp0s8) dan DHCP untuk adapter pertama (enp0s3). IP Statis sesuaikan dengan jaringan yang Anda buat di VirtualBox adapter Host-Only. Misalnya, jika Anda menggunakan IP 192.168.50.1/24 di VirtualBox, maka IP Statis untuk enp0s8 bisa 192.168.50.(isi sesuai keinginan anda)/24.
3. File akan kosong lalu isi dengan konfigurasi berikut:

   ```yaml
   network:
     ethernets:
       enp0s3:
         dhcp4: true
       enp0s8:
         dhcp4: no
         addresses:
           - 192.168.50.10/24
         routes:
           - to: default
             via: 192.168.1.1
         nameservers:
           addresses:
             - 8.8.8.8
             - 8.8.4.4
             - search: []
     version: 2
   ```

4. Simpan perubahan dengan menekan `CTRL + X`, lalu tekan `Y` untuk mengonfirmasi penyimpanan, dan tekan `Enter` untuk keluar dari editor.
5. Terapkan konfigurasi jaringan yang telah Anda buat dengan menjalankan perintah berikut:

   ```bash
   sudo netplan apply
   ```

6. Untuk memastikan konfigurasi jaringan berhasil, jalankan perintah berikut untuk memeriksa alamat IP:

   ```bash
    ip addr show
   ```

   atau

   ```bash
   ip a
   ```

7. Anda seharusnya melihat alamat IP yang telah Anda atur untuk adapter enp0s8.
8. Untuk memastikan koneksi internet berfungsi, coba ping ke alamat IP publik seperti Google DNS:

   ```bash
   ping 8.8.8.8
   ping google.com
   ping polinema.ac.id
   ping youtube.com
   ```

9. Jika Anda menerima balasan, berarti koneksi internet berfungsi dengan baik.
