# **Download dan Instalasi Apache HBase**

## **Langkah 1: Download Apache Hbase**

**Di VM hadoop-primary, hadoop-secondary-1, dan hadoop-secondary-2**

1. Download Apache HBase Stable Release dari [Apache HBase Releases](https://hbase.apache.org/downloads.html).
2. Pili Versi yang stable
3. Ketik perintah berikut untuk mengunduh HBase:
   ```bash
   wget https://archive.apache.org/dist/hbase/2.5.11/hbase-2.5.11-bin.tar.gz
   ```
4. Ekstrak file yang telah diunduh ke dalam direktori yang diinginkan:

   ```bash
   # Ekstrak ke /usr/local/
   sudo tar -xzf hbase-2.5.11-bin.tar.gz -C /usr/local/
   ```

5. Rename Direktori HBase yang telah diekstrak:

   ```bash
   sudo mv /usr/local/hbase-2.5.11 /usr/local/hbase
   ```

6. Ubah kepemilikan direktori HBase ke pengguna saat ini:

   ```bash
   sudo chown -R hadoopuser:hadoopuser /usr/local/hbase
   ```

7. Verifikasi Struktur Direktori HBase:

   ```bash
   ls -la /usr/local/hbase/
   ```

   Output yang diharapkan:

   ```
   drwxr-xr-x  3 hadoopuser hadoopuser 4096 Oct  1 12:00 .
   drwxr-xr-x  4 hadoopuser hadoopuser 4096 Oct  1 12:00 conf
   drwxr-xr-x  2 hadoopuser hadoopuser 4096 Oct  1 12:00 lib
   drwxr-xr-x  2 hadoopuser hadoopuser 4096 Oct  1 12:00 logs
   drwxr-xr-x  2 hadoopuser hadoopuser 4096 Oct  1 12:00 bin
   drwxr-xr-x  2 hadoopuser hadoopuser 4096 Oct  1 12:00 sbin
   drwxr-xr-x  2 hadoopuser hadoopuser 4096 Oct  1 12:00 webapps
   drwxr-xr-x  2 hadoopuser hadoopuser 4096 Oct  1 12:00 zookeeper
   drwxr-xr-x  2 hadoopuser hadoopuser 4096 Oct  1 12:00 hbase-2.5.11-bin.tar.gz
   ```

## **Langkah 2: Konfigurasi Environment HBase**

**Di VM hadoop-primary, hadoop-secondary-1, dan hadoop-secondary-2**

1. Buka file `.bashrc` untuk mengedit:

   ```bash
   nano ~/.bashrc
   ```

2. Tambahkan baris berikut di bagian akhir file `.bashrc`:

   ```yaml
   # Tambahkan baris berikut:
   export HBASE_HOME=/usr/local/hbase
   export PATH=$PATH:$HBASE_HOME/bin:$HBASE_HOME/sbin
   export HBASE_CONF_DIR=$HBASE_HOME/conf
   export HBASE_CLASSPATH=$HADOOP_HOME/etc/hadoop
   ```

3. Simpan dan keluar dari editor (Ctrl + X, Y, Enter).
4. Terapkan perubahan pada `.bashrc`:

   ```bash
   source ~/.bashrc
   ```

5. Verifikasi bahwa variabel lingkungan telah ditambahkan dengan benar:

   ```bash
    echo $HBASE_HOME
    which hbase
    hbase version
   ```

   Output yang diharapkan:

   ```
   /usr/local/hbase
   /usr/local/hbase/bin/hbase
   HBase 2.5.11
   ```

## **Langkah 3: Konfigurasi HBase**

**Di VM hadoop-primary**

1. Buka file env HBase:

   ```bash
   nano $HBASE_HOME/conf/hbase-env.sh
   ```

2. Tambahkan atau ubah baris berikut untuk mengatur Java Home:

   ```bash
    # Java Configuration - JDK 11
    export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

    # ZooKeeper Management - menggunakan eksternal ZooKeeper 3.8.4
    export HBASE_MANAGES_ZK=false

    # Classpath Configuration untuk Hadoop 3.4
    export HBASE_CLASSPATH=$HADOOP_HOME/etc/hadoop:$HADOOP_HOME/share/hadoop/common/*:$HADOOP_HOME/share/hadoop/common/lib/*:$HADOOP_HOME/share/hadoop/hdfs/*:$HADOOP_HOME/share/hadoop/hdfs/lib/*

    # JVM Options untuk HBase 2.5.11
    export HBASE_HEAPSIZE=2048
    export HBASE_OFFHEAPSIZE=1024

    # Master JVM Options - optimized untuk JDK 11
    export HBASE_MASTER_OPTS="-Xms1024m -Xmx2048m -XX:+UseG1GC -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions -XX:G1NewSizePercent=20 -XX:+UnlockDiagnosticVMOptions"

    # RegionServer JVM Options - optimized untuk JDK 11
    export HBASE_REGIONSERVER_OPTS="-Xms2048m -Xmx4096m -XX:+UseG1GC -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions -XX:G1NewSizePercent=20 -XX:+UnlockDiagnosticVMOptions -XX:G1MixedGCLiveThresholdPercent=90"

    # Log Configuration
    export HBASE_LOG_DIR=$HBASE_HOME/logs
    export HBASE_PID_DIR=$HBASE_HOME/pids

    # Security dan Performance untuk JDK 11
    export HBASE_OPTS="-XX:+UseBiasedLocking -XX:BiasedLockingStartupDelay=0 -XX:+OptimizeStringConcat"

    # Hadoop Configuration
    export HADOOP_HOME=/usr/local/hadoop
    export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop

   ```

3. Simpan dan keluar dari editor (Ctrl + X, Y, Enter).
4. Buka file konfigurasi HBase `hbase-site.xml`:

   ```bash
   nano $HBASE_HOME/conf/hbase-site.xml
   ```

5. Tambahkan atau ubah konfigurasi berikut:

   ```xml
   <property>
        <name>hbase.rootdir</name>
        <value>hdfs://hadoop-primary:9000/hbase</value>
    </property>

    <!-- Cluster Configuration -->
    <property>
        <name>hbase.cluster.distributed</name>
        <value>true</value>
    </property>

    <!-- ZooKeeper 3.8.4 Configuration -->
    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>192.168.50.40,192.168.50.50,192.168.50.60</value>
    </property>

    <property>
        <name>hbase.zookeeper.property.clientPort</name>
        <value>2181</value>
    </property>

    <property>
        <name>hbase.zookeeper.property.dataDir</name>
        <value>/usr/local/zookeeper/data</value>
    </property>

    <!-- Master Configuration -->
    <property>
        <name>hbase.master.hostname</name>
        <value>hadoop-primary</value>
    </property>

    <property>
        <name>hbase.master.port</name>
        <value>16000</value>
    </property>

    <property>
        <name>hbase.master.info.port</name>
        <value>16010</value>
    </property>

    <!-- RegionServer Configuration -->
    <property>
        <name>hbase.regionserver.port</name>
        <value>16020</value>
    </property>

    <property>
        <name>hbase.regionserver.info.port</name>
        <value>16030</value>
    </property>

    <!-- WAL Configuration -->
    <property>
        <name>hbase.wal.provider</name>
        <value>filesystem</value>
    </property>

    <!-- Hadoop 3.4 Compatibility -->
    <property>
        <name>hbase.filesystem.replication.override</name>
        <value>true</value>
    </property>

   ```

6. Simpan dan keluar dari editor (Ctrl + X, Y, Enter).

## **Langkah 4: Konfigurasi RegionServer**

**Di VM hadoop-primary**

1. Buka file konfigurasi regionserver
   ```bash
    # Edit file regionservers untuk mendefinisikan RegionServer nodes
    nano $HBASE_HOME/conf/regionservers
   ```
2. Tambahkan ip dari setiap RegionServer yang akan digunakan dalam cluster HBase:

   ```yaml
   192.168.50.20
   192.168.50.30
   ```

3. Simpan dan keluar dari editor (Ctrl + X, Y, Enter).
4. Tambahkan konfigurasi untuk backup master
   ```bash
   # Buat file backup-masters untuk HA
   nano $HBASE_HOME/conf/backup-masters
   ```
5. Tambahkan IP node yang akan digunakan sebagai backup master:

   ```yaml
   192.168.50.20
   ```

6. Simpan dan keluar dari editor (Ctrl + X, Y, Enter).
7. Salin konfigurasi HBase untuk node RegionServer lainnya:

   ```bash
    # Salin konfigurasi ke hadoop-secondary-1
    scp -r $HBASE_HOME/conf/* hadoopuser@192.168.50.20:$HBASE_HOME/conf/

    # Salin konfigurasi ke hadoop-secondary-2
    scp -r $HBASE_HOME/conf/* hadoopuser@192.168.50.30:$HBASE_HOME/conf/
   ```

8. Verifikasi bahwa konfigurasi telah disalin dengan benar:

   ```bash
    ssh hadoopuser@192.168.50.20 "ls -la $HBASE_HOME/conf/"
    ssh hadoopuser@192.168.50.30 "ls -la $HBASE_HOME/conf/"
   ```

   Output yang diharapkan:

   ```
   drwxr-xr-x  3 hadoopuser hadoopuser 4096 Oct  1 12:00 .
   drwxr-xr-x  4 hadoopuser hadoopuser 4096 Oct  1 12:00 conf
   drwxr-xr-x  2 hadoopuser hadoopuser 4096 Oct  1 12:00 lib
   drwxr-xr-x  2 hadoopuser hadoopuser 4096 Oct  1 12:00 logs
   drwxr-xr-x  2 hadoopuser hadoopuser 4096 Oct  1 12:00 bin
   drwxr-xr-x  2 hadoopuser hadoopuser 4096 Oct  1 12:00 sbin
   drwxr-xr-x  2 hadoopuser hadoopuser 4096 Oct  1 12:00 webapps
   drwxr-xr-x  2 hadoopuser hadoopuser 4096 Oct  1 12:00 zookeeper
   ```

9. Membuat Direktori untuk menyimpan data HBase di HDFS:

   ```bash
   # Buat direktori HBase di HDFS
    hdfs dfs -mkdir -p /hbase
   ```

10. Atur izin direktori HBase di HDFS:

    ```bash
    hdfs dfs -chown hadoopuser:hadoopuser /hbase
    hdfs dfs -chmod 755 /hbase
    ```

11. Verifikasi direktori HBase di HDFS:

    ```bash
    hdfs dfs -ls /
    hdfs dfs -ls /hbase
    ```

## **Langkah 5: Menjalankan HBase**

**Di VM hadoop-primary**

1. Verifikasi Pre-requisites:

   ```bash
    # Pastikan Hadoop 3.4 cluster sudah berjalan
    jps | grep -E "(NameNode|DataNode|ResourceManager|NodeManager)"
   ```

2. Verifikasi bahwa ZooKeeper sudah berjalan:

   ```bash
    # Pastikan ZooKeeper 3.8.4 cluster sudah berjalan
    for server in 192.168.50.40 192.168.50.50 192.168.50.60; do
        echo "Checking ZooKeeper 3.8.4 on $server:"
        echo "ruok" | nc $server 2181
        echo "stat" | nc $server 2181 | head -5
    done
   ```

3. Jalankan HBase:

   ```bash
    # Jalankan HBase Master
    start-hbase.sh

    Tunggu beberapa saat sekitar 30 detik hingga HBase Master dan RegionServer berjalan

   ```

4. Verifikasi bahwa HBase Master dan RegionServer telah berjalan:

   ```bash
    jps
   ```

   ```bash
   hbase version
   ```

5. Verifikasi proses Java

   ```bash
   # Di hadoop-primary, seharusnya ada HMaster
   jps | grep HMaster
   ```

   ```bash
   # Di hadoop-secondary nodes, seharusnya ada HRegionServer
   ssh hadoopuser@192.168.50.20 "jps | grep HRegionServer"
   ssh hadoopuser@192.168.50.30 "jps | grep HRegionServer"
   ```

6. Untuk verifikasi langsung di setiap Node

   ```bash
    # Di Hadoop-Secondary-1
    jps | grep HRegionServer
   ```

   ```bash
    # Di Hadoop-Secondary-2
    jps | grep HRegionServer
   ```

7. Verifikasi Status Hbase Master:

   ```bash
   # Cek status HBase
    hbase shell <<EOF
    status
    status 'simple'
    status 'summary'
    status 'detailed'
    version
    exit
    EOF
   ```

8. Test Operasi HBase:

   ```bash
    # Test lengkap HBase 2.5.11 functionality
    hbase shell <<EOF
    # Cek version
    version

    # List semua tabel
    list

    # Buat tabel test dengan column families
    create 'test_table_2511', 'cf1', 'cf2'

    # Lihat struktur tabel
    describe 'test_table_2511'

    # Insert data
    put 'test_table_2511', 'row1', 'cf1:col1', 'value1'
    put 'test_table_2511', 'row1', 'cf1:col2', 'value2'
    put 'test_table_2511', 'row1', 'cf2:col1', 'value3'
    put 'test_table_2511', 'row2', 'cf1:col1', 'value4'

    # Scan tabel
    scan 'test_table_2511'

    # Get data spesifik
    get 'test_table_2511', 'row1'

    # Count rows
    count 'test_table_2511'

    # Test HBase 2.5.11 features - namespace
    create_namespace 'test_ns'
    list_namespace

    # Delete data
    delete 'test_table_2511', 'row1', 'cf1:col1'

    # Scan lagi untuk verifikasi
    scan 'test_table_2511'

    # Disable dan drop tabel
    disable 'test_table_2511'
    drop 'test_table_2511'

    # Clean up namespace
    drop_namespace 'test_ns'

    # Verifikasi tabel sudah terhapus
    list

    exit
    EOF
   ```

9. Verifikasi HDFS dengan Hadoop 3.4:

   ```bash
   # Test HDFS functionality
   hdfs dfs -mkdir /test_hbase2511
   hdfs dfs -put /etc/hosts /test_hbase2511/
   hdfs dfs -ls /test_hbase2511/
   hdfs dfs -cat /test_hbase2511/hosts

   # Cek status HDFS Hadoop 3.4
   hdfs dfsadmin -report
   hdfs dfsadmin -safemode get
   hdfs version
   ```

10. Verifikasi Yarn

    ```bash
    # Cek YARN nodes
    yarn node -list
    yarn node -status hadoop-secondary-1:8042
    yarn node -status hadoop-secondary-2:8042

    # Cek aplikasi yang berjalan
    yarn application -list
    yarn version
    ```

11. Verifikasi Zookeeper dengan cluster HBase

    ```bash
    # Test semua ZooKeeper 3.8.4 nodes
    for server in 192.168.50.40 192.168.50.50 192.168.50.60; do
        echo "=== Testing ZooKeeper 3.8.4 on $server ==="
        echo "ruok" | nc $server 2181
        echo "stat" | nc $server 2181 | head -10
        echo "conf" | nc $server 2181 | grep -E "(clientPort|dataDir|serverId|version)"
        echo "version" | nc $server 2181
        echo
    done
    ```

12. Verifikasi Integreasi HBase dengan Zookeeper

    ```bash
    # Cek ZooKeeper dari HBase shell
    hbase shell <<EOF
    zk_dump
    exit
    EOF

    # Manual check ZooKeeper znodes untuk HBase 2.5.11
    zkCli.sh -server 192.168.50.40:2181 <<EOF
    ls /
    ls /hbase
    ls /hbase/rs
    ls /hbase/master
    quit
    EOF
    ```

## **Langkah 6: Verifikasi Haddop, Yarn, dan HBase Web UI**

1. Setelah semua layanan berjalan, Anda dapat mengakses web interface:

   - Hadoop NameNode (3.4): http://192.168.50.10:9870
   - YARN ResourceManager: http://192.168.50.10:8088
   - HBase Master (2.5.11): http://192.168.50.10:16010
   - HBase RegionServer 1: http://192.168.50.20:16030
   - HBase RegionServer 2: http://192.168.50.30:16030
