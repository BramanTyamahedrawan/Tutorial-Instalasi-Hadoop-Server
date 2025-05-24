# **Instalasi Linux di Virtual Machine**

## **Langkah 1: Buka VirtualBox**

1. Buka VirtualBox yang telah Anda instal sebelumnya.
2. Disini saya pakai Ubuntu Server Terbaru

## **Langkah 2: Install Ubuntu Server**

1. Pilih VM "Hadoop-Template" yang telah Anda buat.
2. Klik pada tombol "Start" atau "Mulai" untuk memulai mesin virtual.
3. Klik try install Ubuntu Server.
4. Proses Instalasi Ubuntu Server:
   - Pilih bahasa yang diinginkan (misalnya, "English").
   - Keyboard layout: Pilih "English (US)" atau sesuai preferensi Anda.
   - Pilih opsi "Install Ubuntu Server".
5. Network Configuration:
   - Konfirmasi enp0s3 sebagai interface jaringan.
   - Pilih "Configure network automatically using DHCP" untuk mendapatkan alamat IP secara otomatis.
   - Adapter kedua (enp0s8) akan dikonfigurasi nanti.
   - Klik "Done" untuk melanjutkan.
6. Proxy Configuration:
   - Jika Anda tidak menggunakan proxy, biarkan kosong dan tekan Enter.
   - Klik "Done" untuk melanjutkan.
7. Mirror Configuration:
   - Gunakan Default Mirror.
   - Pilih "Yes" untuk mengkonfigurasi mirror.
   - Klik "Done" untuk melanjutkan.
8. Storage Configuration:
   - Pilih "Use an entire disk" untuk menginstal Ubuntu pada hard disk virtual.
   - Jangan pilih opsi LVM.
   - Uncheck opsi LVM jika opsi LVM tercentang.
   - Klik "Done" untuk melanjutkan.
9. Disk Partitioning:
   - Review partisi yang akan dibuat.
   - Pastikan partisi sesuai dengan yang diinginkan.
   - Klik "Done" untuk melanjutkan.
   - Konfirmasi COntinue dengan menekan "Continue".
10. User Setup:
    - Masukkan nama pengguna (misalnya, "Hadoop User").
    - Masukkan nama server (misalnya, "hadoop-template").
    - Masukkan username (misalnya, "hadoopuser").
    - Masukkan password untuk akun pengguna.
    - Konfirmasi password.
    - Klik "Done" untuk melanjutkan.
11. SSH Setup:
    - Pilih "Install OpenSSH server" untuk mengaktifkan akses SSH.
    - Klik "Done" untuk melanjutkan.
12. Featured Server Snaps:
    - Tidak perlu memilih snap untuk instalasi awal.
    - Pilih "No snaps selected" untuk melewati instalasi snap.
    - Klik "Done" untuk melanjutkan.
13. Proses instalasi akan dimulai. Tunggu hingga selesai.
14. Setelah instalasi selesai, pilih "Reboot now" untuk me-restart mesin virtual.
15. Jika anda melihat gambar seperti ini maka press enter

    ![ENTER](<Screenshot 2025-05-20 125218.png>)

    Ini adalah pesan umum yang muncul ketika sistem operasi mencoba untuk mengeluarkan (unmount) media instalasi (ISO) setelah instalasi selesai, tetapi gagal melakukannya.

16. Jika masih menampilkan pesan error yang sama, Anda bisa:


    - Matikan VM (shutdown)
    - Di VirtualBox Manager, pilih VM Anda
    - Klik "Settings"
    - Pilih "Storage"
    - Pada Controller IDE, klik pada file ISO yang terpasang
    - Klik ikon disk kecil di sebelah kanan dan pilih "Remove Disk from Virtual Drive"
    - Klik "OK"
    - Jalankan kembali VM

17. Setelah reboot, Anda akan melihat prompt login.
18. Masukkan username dan password yang telah Anda buat sebelumnya untuk masuk ke sistem.
19. Setelah masuk, Anda dapat mulai menggunakan Ubuntu Server di mesin virtual Anda.
20. Untuk langkah 15 dan 16 adalah langkah yang kadang muncul, jika tidak muncul maka tidak perlu melakukan langkah tersebut.
