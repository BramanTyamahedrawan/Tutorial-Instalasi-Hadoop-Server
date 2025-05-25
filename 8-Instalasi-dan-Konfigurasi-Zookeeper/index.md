# **Instalasi dan Konfigurasi Zookeeper**

## **Langkah 1: Instalasi Zookeeper**

**Di VM Zookeeper-1, Zookeeper-2, dan Zookeeper-3:**

1. Download Zookeeper

   ```bash
   wget https://archive.apache.org/dist/zookeeper/zookeeper-3.8.4/apache-zookeeper-3.8.4-bin.tar.gz
   ```

2. Ekstrak file yang telah diunduh

   ```bash
   sudo tar -xzf apache-zookeeper-3.7.2-bin.tar.gz -C /usr/local/
   ```

3. Rename Direktori dan Pindahkan zookeeper

   ```bash
   sudo mv /usr/local/apache-zookeeper-3.7.2-bin /usr/local/zookeeper
   ```

4. Ubah kepemilikan direktori zookeeper

   ```bash
   sudo chown -R hadoopuser:hadoopuser /usr/local/zookeeper
   ```

## **Langkah 2: Konfigurasi Environtment Variable Zookeeper**

**Di VM Zookeeper-1, Zookeeper-2, dan Zookeeper-3:**

1. Edit Environment Variables

   ```bash
   nano ~/.bashrc
   ```

2. Tambahkan baris berikut di akhir file `.bashrc`

   ```yaml
   export ZOOKEEPER_HOME=/usr/local/zookeeper
   export PATH=$PATH:$ZOOKEEPER_HOME/bin
   export ZOO_LOG_DIR=/usr/local/zookeeper/logs
   ```

3. Simpan dan keluar dari editor, kemudian jalankan perintah berikut untuk menerapkan perubahan

   ```bash
    source ~/.bashrc
   ```

4. Verifikasi environment variables

   ```bash
   echo $ZOOKEEPER_HOME
   echo $PATH | grep zookeeper
   ```

5. Buat direktori untuk menyimpan data Zookeeper

   ```bash
   mkdir -p /usr/local/zookeeper/data
   ```

6. Buat direktori untuk menyimpan log Zookeeper

   ```bash
   mkdir -p /usr/local/zookeeper/logs
   ```

7. Set permission direktori data dan logs

   ```bash
   chmod 755 /usr/local/zookeeper/data
   chmod 755 /usr/local/zookeeper/logs
   ```

8. Verifikasi Direktori

   ```bash
   ls -la /usr/local/zookeeper/data
   ls -la /usr/local/zookeeper/logs
   ```

## **Langkah 3: Konfigurasi Server myID Zookeeper**

**Di VM Zookeeper-1 (192.168.50.40)**

1. Buat file konfigurasi `myid`

   ```bash
   echo "1" > /usr/local/zookeeper/data/myid
   cat /usr/local/zookeeper/data/myid  # Verifikasi
   ```

**Di VM Zookeeper-2 (192.168.50.50)**

1. Buat file konfigurasi `myid`

   ```bash
   echo "2" > /usr/local/zookeeper/data/myid
   cat /usr/local/zookeeper/data/myid  # Verifikasi
   ```

**Di VM Zookeeper-3 (192.168.50.60)**

1. Buat file konfigurasi `myid`

   ```bash
   echo "3" > /usr/local/zookeeper/data/myid
   cat /usr/local/zookeeper/data/myid  # Verifikasi
   ```

## **Langkah 4: Konfigurasi File Zookeeper**

**Di VM Zookeeper-1, Zookeeper-2, dan Zookeeper-3:**

1. Edit file konfigurasi `zoo.cfg`

   ```bash
   nano /usr/local/zookeeper/conf/zoo.cfg
   ```

2. Edit file `zoo.cfg` dengan menambahkan konfigurasi berikut:

   ```yaml
    # Basic Configuration
    tickTime=2000
    dataDir=/usr/local/zookeeper/data
    dataLogDir=/usr/local/zookeeper/logs
    clientPort=2181
    clientPortAddress=0.0.0.0

    # Cluster Configuration

    initLimit=10
    syncLimit=5
    maxClientCnxns=100

    # Performance Optimization

    preAllocSize=65536
    snapCount=100000
    autopurge.snapRetainCount=5
    autopurge.purgeInterval=24

    # Server Configuration (3 Node Cluster)

    server.1=192.168.50.40:2888:3888
    server.2=192.168.50.50:2888:3888
    server.3=192.168.50.60:2888:3888

    # Security Configuration

    admin.enableServer=false
    4lw.commands.whitelist=ruok,stat,srvr,mntr,conf,cons,envi,isro

   ```

3. Simpan dan keluar dari editor
4. Verifikasi file konfigurasi

   ```bash
   cat /usr/local/zookeeper/conf/zoo.cfg
   ```

5. Kofigurasi Java untuk Zookeeper

   ```bash
   # Edit file java.env untuk optimasi JVM
   nano /usr/local/zookeeper/conf/java.env
   ```

6. Tambahkan dan edit baris berikut di akhir file `java.env`

   ```yaml
   export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
   export JVMFLAGS="-Xmx2G -Xms2G -XX:+UseG1GC -XX:MaxGCPauseMillis=200"
   export ZOO_LOG4J_PROP="INFO,ROLLINGFILE"
   ```

7. Simpan dan keluar dari editor

## **Langkah 5: Menjalankan Zookeeper**

**Di VM Zookeeper-1, Zookeeper-2, dan Zookeeper-3:**

1. Jalankan Zookeeper

   ```bash
   # Start ZooKeeper di setiap node (jalankan secara berurutan)
   zkServer.sh start
   ```

2. Verifikasi status Zookeeper

   ```bash
    zkServer.sh status
   ```

3. Verifikasi proses Zookeeper

   ```bash
   # Verifikasi proses berjalan
   jps | grep QuorumPeerMain
   ```

4. Verifikasi log Zookeeper

   ```bash
   # Cek log untuk memastikan tidak ada error
   tail -f /usr/local/zookeeper/logs/zookeeper.out
   ```

5. Verifikasi cluster Zookeeper

   ```bash
    # Test konektivitas dari setiap node
    echo "ruok" | nc localhost 2181
    echo "stat" | nc localhost 2181

    # Test dari node lain
    echo "ruok" | nc 192.168.50.40 2181
    echo "ruok" | nc 192.168.50.50 2181
    echo "ruok" | nc 192.168.50.60 2181
   ```
