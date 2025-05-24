# **Download dan Install Java**

## **Langkah 1: Download Java**

1. Lakukan Download Java pada **SETIAP VM (hadoop-primary, hadoop-secondary-1, dst ....)**
   - Disini kita akan menggunakan OpenJDK 11 sebagai contoh, namun Anda dapat memilih versi lain sesuai kebutuhan.
2. pada setiap VM, lakukan update
   ```bash
   sudo apt update
   ```
3. Kemudian lakukan install Java dengan perintah berikut:
   ```bash
   sudo apt install -y openjdk-11-jdk
   ```
4. Setelah instalasi selesai, verifikasi instalasi Java dengan perintah:
   ```bash
   java -version
   ```
5. Anda seharusnya melihat output yang menunjukkan versi Java yang telah diinstal, seperti:
   ```
   openjdk version "11.0.11" 2021-04-20
   OpenJDK Runtime Environment (build 11.0.11+9-Ubuntu-0ubuntu2)
   OpenJDK 64-Bit Server VM (build 11.0.11+9-Ubuntu-0ubuntu2, mixed mode, sharing)
   ```

## **Langkah 2: Set Environment Variables**

1. Setelah Java terinstal, Anda perlu menambahkan JAVA_HOME ke dalam environment variables.
2. Ketik perintah berikut
   ```bash
   echo 'export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64' >> ~/.bashrc
   ```
3. Kemudian, jalankan perintah berikut untuk add PATH ke dalam environment variables:
   ```bash
   echo 'export PATH=$PATH:$JAVA_HOME/bin' >> ~/.bashrc
   ```
4. Setelah itu, muat ulang file `.bashrc` untuk menerapkan perubahan:

   ```bash
   source ~/.bashrc
   ```

5. Verifikasi bahwa JAVA_HOME telah ditetapkan dengan benar:
   ```bash
   echo $JAVA_HOME
   ```
6. Anda seharusnya melihat output yang menunjukkan path ke direktori Java, seperti:
   ```
   /usr/lib/jvm/java-11-openjdk-amd64
   ```
7. Untuk memastikan bahwa PATH telah diperbarui, Anda dapat menjalankan:
   ```bash
   echo $PATH
   ```
8. Anda seharusnya melihat path ke direktori bin Java di dalam output PATH.
