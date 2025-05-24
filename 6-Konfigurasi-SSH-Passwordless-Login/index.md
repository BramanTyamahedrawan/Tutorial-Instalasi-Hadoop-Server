# **Konfigurasi SSH Passwordless Login**

## **Langkah 1: Generate SSH Key Pair**

**Di Hadoop Primary VM (hadoop-primary)**

1. Generate SSH key pair dengan perintah berikut:
   ```bash
    ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
   ```
2. Perintah ini akan membuat sepasang kunci SSH (private key dan public key) di direktori `~/.ssh/` tanpa passphrase.
3. Pastikan untuk tidak menimpa kunci yang sudah ada jika Anda telah membuat kunci sebelumnya.
4. Setelah menjalankan perintah ini, Anda akan melihat output yang menunjukkan lokasi kunci yang telah dibuat.
5. Contoh output:
   ```
   Generating public/private rsa key pair.
   Your identification has been saved in /home/username/.ssh/id_rsa.
   Your public key has been saved in /home/username/.ssh/id_rsa.pub.
   The key fingerprint is:
   SHA256:XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX username@hadoop-primary
   The key's randomart image is:
   +---[RSA 3072]----+
   |                 |
   |                 |
   |                 |
   |                 |
   |        S        |
   |       . o .     |
   |      o + = .    |
   |     . + O = o   |
   |      E+o=+o.    |
   +----[SHA256]-----+
   ```

## **Langkah 2: Tambahkan Key ke Authorized Keys Local dan Salin Key ke Node Lain**

**Di Hadoop Primary VM (hadoop-primary)**

1. Tambahkan public key ke file `authorized_keys` untuk mengizinkan akses SSH tanpa password:
   ```bash
   cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
   ```
2. Pastikan file `authorized_keys` memiliki izin yang benar:
   ```bash
    chmod 600 ~/.ssh/authorized_keys
   ```
3. Verifikasi bahwa public key telah ditambahkan dengan benar:
   ```bash
   cat ~/.ssh/authorized_keys
   ```
4. Salin public key ke setiap node Hadoop lainnya (hadoop-secondary-1, hadoop-secondary-2, dst.):

   ```bash
   scp ~/.ssh/id_rsa.pub hadoopuser@hadoop-secondary-1:~/.ssh/id_rsa.pub_primary
   scp ~/.ssh/id_rsa.pub hadoopuser@hadoop-secondary-2:~/.ssh/id_rsa.pub_primary
   scp ~/.ssh/id_rsa.pub hadoopuser@zookeeper-1:~/.ssh/id_rsa.pub_primary
   scp ~/.ssh/id_rsa.pub hadoopuser@zookeeper-2:~/.ssh/id_rsa.pub_primary
   scp ~/.ssh/id_rsa.pub hadoopuser@zookeeper-3:~/.ssh/id_rsa.pub_primary
   ```

   # Pastikan untuk memasukkan password saat diminta

## **Langkah 3: Tambahkan Public Key ke Authorized Keys di Setiap Node**

**Di Setiap Node VM (hadoop-secondary-1, hadoop-secondary-2, zookeeper-1, zookeeper-2, zookeeper-3)**

1. Buat direktori `.ssh` jika belum ada:
   ```bash
   mkdir -p ~/.ssh
   ```
2. Tambahkan public key dari primary node ke file `authorized_keys`:
   ```bash
    cat ~/.ssh/id_rsa.pub_primary >> ~/.ssh/authorized_keys
   ```
3. Pastikan file `authorized_keys` memiliki izin yang benar:
   ```bash
   chmod 600 ~/.ssh/authorized_keys
   ```

## **Langkah 4: Verifikasi SSH Passwordless Login**

**Di Hadoop Primary VM (hadoop-primary)**

1. Coba SSH ke setiap node tanpa memasukkan password:
   ```bash
    ssh hadoopuser@hadoop-secondary-1 "hostname"
    ssh hadoopuser@hadoop-secondary-2 "hostname"
    ssh hadoopuser@zookeeper-1 "hostname"
    ssh hadoopuser@zookeeper-2 "hostname"
    ssh hadoopuser@zookeeper-3 "hostname"
   ```
2. Jika konfigurasi berhasil, Anda akan masuk ke node tersebut tanpa diminta memasukkan password.
