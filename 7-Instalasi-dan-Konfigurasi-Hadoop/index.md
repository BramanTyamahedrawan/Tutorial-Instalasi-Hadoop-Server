# **Download dan Instalasi Hadoop**

**Disini saya menggunakan Hadoop 3.4 sebagai contoh, namun Anda dapat memilih versi lain sesuai kebutuhan.**

## **Langkah 1: Download Hadoop**

1. Lakukan Download Hadoop pada **SETIAP VM (hadoop-primary, hadoop-secondary-1, dst ....)**
   - Anda dapat mengunduh Hadoop dari situs resmi Apache Hadoop atau menggunakan perintah `wget` untuk mengunduhnya langsung ke VM.
   - Contoh perintah untuk mengunduh Hadoop 3.4.0:
     ```bash
     wget https://downloads.apache.org/hadoop/common/hadoop-3.4.0/hadoop-3.4.0.tar.gz
     ```
2. Setelah selesai mengunduh, ekstrak file tar.gz dengan perintah:
   ```bash
   tar -xzf hadoop-3.4.0.tar.gz
   ```
3. Pindahkan direktori Hadoop yang diekstrak ke direktori yang sesuai, misalnya `/usr/local/`:
   ```bash
   sudo mv hadoop-3.4.0 /usr/local/
   ```
4. Pindahkan direktori yang telah diekstrak ke direktori yang lebih mudah diakses, seperti `/usr/local/hadoop`:
   ```bash
   sudo mv /usr/local/hadoop-3.4.0 /usr/local/hadoop
   ```
5. Ubah kepemilikan direktori Hadoop agar sesuai dengan pengguna yang akan menjalankan Hadoop (misalnya `hadoopuser`):
   ```bash
   sudo chown -R hadoopuser:hadoopuser /usr/local/hadoop
   ```

## **Langkah 2: Set Environment Variables untuk Hadoop**

1.  Lakukan penambahan environment variables untuk Hadoop dengan mengedit file `.bashrc`:
    lakukan satu satu perintah berikut pada setiap VM:

    ```bash
    echo 'export HADOOP_HOME=/usr/local/hadoop' >> ~/.bashrc
    ```

    ```bash
    echo 'export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin' >> ~/.bashrc
    ```

    ```bash
    echo 'export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop' >> ~/.bashrc
    ```

    ```bash
    echo 'export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64' >> ~/.bashrc
    ```

    ```bash
    echo 'export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native"' >> ~/.bashrc
    ```

2.  Setelah menambahkan environment variables, muat ulang file `.bashrc` untuk menerapkan perubahan:

    ```bash
    source ~/.bashrc
    ```

3.  Verifikasi bahwa environment variables telah ditetapkan dengan benar:

    ```bash
     echo $HADOOP_HOME
    ```

    ```bash
    java -version  # Memastikan OpenJDK 11 terdeteksi
    hadoop version # Memastikan Hadoop 3.4.0 terinstal dengan benar
    ```

## **Langkah 3: Konfigurasi Hadoop**

**Pada Hadoop Primary VM (hadoop-primary)**

1. Edit file hadoop-env.sh untuk mengatur JAVA_HOME:

   ```bash
   nano $HADOOP_HOME/etc/hadoop/hadoop-env.sh
   ```

2. Tambahkan atau ubah baris berikut:

   ```yaml
   export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
   export HADOOP_HEAPSIZE_MAX=1g
   ```

3. Simpan dan keluar dari editor.
4. Edit file core-site.xml untuk mengatur konfigurasi dasar Hadoop:

   ```bash
   nano $HADOOP_HOME/etc/hadoop/core-site.xml
   ```

5. Tambahkan konfigurasi berikut:

   ```xml
    <configuration>
         <property>
              <name>fs.defaultFS</name>
              <value>hdfs://hadoop-primary:9000</value>
         </property>
         <property>
              <name>hadoop.tmp.dir</name>
              <value>/usr/local/hadoop/tmp</value>
         </property>
         <property>
              <name>dfs.namenode.rpc-bind-host</name>
              <value>0.0.0.0</value>
         </property>
   <configuration>
   ```

6. Simpan dan keluar dari editor.
7. Edit file hdfs-site.xml untuk mengatur konfigurasi HDFS:

   ```bash
   nano $HADOOP_HOME/etc/hadoop/hdfs-site.xml
   ```

8. Tambahkan konfigurasi berikut:

   ```xml
   <configuration>
        <property>
            <name>dfs.replication</name>
            <value>2</value>
        </property>
        <property>
            <name>dfs.namenode.name.dir</name>
            <value>/usr/local/hadoop/hdfs/namenode</value>
        </property>
        <property>
            <name>dfs.datanode.data.dir</name>
            <value>/usr/local/hadoop/hdfs/datanode</value>
        </property>
        <property>
            <name>dfs.namenode.http-address</name>
            <value>hadoop-primary:9870</value>
        </property>
        <property>
            <name>dfs.namenode.http-bind-host</name>
            <value>0.0.0.0</value>
        </property>
        <property>
            <name>dfs.namenode.rpc-address</name>
            <value>hadoop-primary:9000</value>
        </property>
        <property>
            <name>dfs.namenode.rpc-bind-host</name>
            <value>0.0.0.0</value>
        </property>

    </configuration>
   ```

9. Simpan dan keluar dari editor.
10. Edit file mapred-site.xml untuk mengatur konfigurasi MapReduce:

    ```bash
    nano $HADOOP_HOME/etc/hadoop/mapred-site.xml
    ```

11. Tambahkan konfigurasi berikut:

    ```xml
    <configuration>
        <property>
            <name>mapreduce.framework.name</name>
            <value>yarn</value>
        </property>
        <property>
            <name>mapreduce.application.classpath</name>
            <value>$HADOOP_HOME/share/hadoop/mapreduce/*:$HADOOP_HOME/share/hadoop/mapreduce/lib/\*</value>
        </property>
        <property>
            <name>mapreduce.jobhistory.address</name>
            <value>hadoop-primary:10020</value>
        </property>
        <property>
            <name>mapreduce.jobhistory.webapp.address</name>
            <value>hadoop-primary:19888</value>
        </property>
        <property>
            <name>mapreduce.jobhistory.bind-host</name>
            <value>0.0.0.0</value>
        </property>
        <property>
            <name>yarn.app.mapreduce.am.env</name>
            <value>HADOOP_MAPRED_HOME=$HADOOP_HOME</value>
        </property>
        <property>
            <name>mapreduce.map.env</name>
            <value>HADOOP_MAPRED_HOME=$HADOOP_HOME</value>
        </property>
        <property>
            <name>mapreduce.reduce.env</name>
            <value>HADOOP_MAPRED_HOME=$HADOOP_HOME</value>
        </property>
    </configuration>
    ```

12. Simpan dan keluar dari editor.
13. Edit file yarn-site.xml untuk mengatur konfigurasi YARN:

    ```bash
    nano $HADOOP_HOME/etc/hadoop/yarn-site.xml
    ```

14. Tambahkan konfigurasi berikut:

    ```xml
    <configuration>
        <property>
            <name>yarn.resourcemanager.hostname</name>
            <value>hadoop-primary</value>
        </property>
        <property>
            <name>yarn.resourcemanager.bind-host</name>
            <value>0.0.0.0</value>
        </property>
        <property>
            <name>yarn.resourcemanager.address</name>
            <value>hadoop-primary:8032</value>
        </property>
        <property>
            <name>yarn.resourcemanager.scheduler.address</name>
            <value>hadoop-primary:8030</value>
        </property>
        <property>
            <name>yarn.resourcemanager.resource-tracker.address</name>
            <value>hadoop-primary:8031</value>
        </property>
        <property>
            <name>yarn.resourcemanager.admin.address</name>
            <value>hadoop-primary:8033</value>
        </property>
        <property>
            <name>yarn.resourcemanager.webapp.address</name>
            <value>hadoop-primary:8088</value>
        </property>
        <property>
            <name>yarn.nodemanager.vmem-check-enabled</name>
            <value>false</value>
        </property>
        <property>
            <name>yarn.nodemanager.aux-services</name>
            <value>mapreduce_shuffle</value>
        </property>
    </configuration>

    ```

15. Simpan dan keluar dari editor.
16. Edit file workers untuk menentukan node worker Hadoop:

    ```bash
    nano $HADOOP_HOME/etc/hadoop/workers
    ```

17. Tambahkan nama host dari setiap node worker:

    ```plaintext
    hadoop-secondary-1
    hadoop-secondary-2
    ```

18. Simpan dan keluar dari editor.
19. Salin file konfigurasi ke setiap node worker (hadoop-secondary-1, hadoop-secondary-2):

    ```bash
    scp $HADOOP_HOME/etc/hadoop/* hadoopuser@hadoop-secondary-1:$HADOOP_HOME/etc/hadoop/
    scp $HADOOP_HOME/etc/hadoop/* hadoopuser@hadoop-secondary-2:$HADOOP_HOME/etc/hadoop/
    ```

## **Langkah 4: Format HDFS**

1.  Buat direktori untuk HDFS Namenode:
    **Pada Hadoop Primary VM (hadoop-primary)**

        ```bash
        mkdir -p /usr/local/hadoop/hdfs/namenode
        mkdir -p /usr/local/hadoop/tmp
        ```

2.  Buat direktori untuk HDFS datanode:
    **Pada Hadoop Secondary VM (hadoop-secondary-1, hadoop-secondary-2)**

        ```bash
        ssh hadoopuser@hadoop-secondary-1 "mkdir -p /usr/local/hadoop/hdfs/datanode /usr/local/hadoop/tmp"
        ssh hadoopuser@hadoop-secondary-2 "mkdir -p /usr/local/hadoop/hdfs/datanode /usr/local/hadoop/tmp"
        ```

3.  Ubah kepemilikan direktori HDFS agar sesuai dengan pengguna yang akan menjalankan Hadoop (misalnya `hadoopuser`):
    **Pada Hadoop Primary VM (hadoop-primary)**

    ```bash
    sudo chown -R hadoopuser:hadoopuser /usr/local/hadoop
    ```

    **Pada Hadoop Secondary VM (hadoop-secondary-1, hadoop-secondary-2)**

    ```bash
    ssh hadoopuser@hadoop-secondary-1 "sudo chown -R hadoopuser:hadoopuser /usr/local/hadoop"
    ssh hadoopuser@hadoop-secondary-2 "sudo chown -R hadoopuser:hadoopuser /usr/local/hadoop"
    ```

4.  Format HDFS Namenode:
    **Pada Hadoop Primary VM (hadoop-primary)**

    ```bash
    hdfs namenode -format
    ```

    **(CATATAN PENTING: Format HDFS hanya dilakukan sekali saat pertama kali setup. Jangan diulang karena akan menghapus semua data di HDFS.)**

## **Langkah 5: Mulai Hadoop**

1.  Mulai HDFS Namenode dan Datanode:
    **Pada Hadoop Primary VM (hadoop-primary)**

    ```bash
    start-dfs.sh
    ```

2.  Mulai YARN ResourceManager dan NodeManager:
    **Pada Hadoop Primary VM (hadoop-primary)**

    ```bash
    start-yarn.sh
    ```

3.  Verifikasi bahwa Hadoop telah berjalan dengan baik:
    **Pada Hadoop Primary VM (hadoop-primary)**

    ```bash
    jps
    ```

    Anda seharusnya melihat output yang menunjukkan proses-proses berikut:

    ```
    Jps
    NameNode
    DataNode
    ResourceManager
    NodeManager
    SecondaryNameNode
    ```

4.  (Optional) Mulai History Server untuk MapReduce:
    **Pada Hadoop Primary VM (hadoop-primary)**

    ```bash
    mr-jobhistory-daemon.sh start historyserver
    ```

    atau

    ```bash
    mapred --daemon start historyserver
    ```

5.  Verifikasi bahwa History Server telah berjalan:

    ```bash
    jps
    ```

    Anda seharusnya melihat output yang menunjukkan proses `JobHistoryServer`.

6.  Buka antarmuka web Hadoop untuk memverifikasi bahwa semuanya berjalan dengan baik:
    - Buka browser dan akses:
      - HDFS Namenode: `http://hadoop-primary:9870`
      - YARN ResourceManager: `http://hadoop-primary:8088`
7.  Anda seharusnya melihat antarmuka web yang menunjukkan status HDFS dan YARN.

## **Langkah 6: Verifikasi Instalasi**

1.  Buat direktori di HDFS untuk menguji instalasi:
    **Pada Hadoop Primary VM (hadoop-primary)**

    ```bash

    hdfs dfs -mkdir /user
    hdfs dfs -mkdir /user/hadoopuser
    hdfs dfs -put $HADOOP_HOME/etc/hadoop/core-site.xml /user/hadoopuser/
    hdfs dfs -ls /user/hadoopuser
    hdfs dfsadmin -report
    ```

2.  Verifikasi Yarn
    **Pada Hadoop Primary VM (hadoop-primary)**

    ```bash
    yarn node -list
    yarn application -list
    ```

3.  Verifikasi MapReduce
    **Pada Hadoop Primary VM (hadoop-primary)**

    ```bash
    mapred job -list
    ```

4.  Jika anda tidak bisa membuka antarmuka web Hadoop, pastikan firewall tidak memblokir port yang digunakan oleh Hadoop (9870 untuk HDFS dan 8088 untuk YARN).
5.  Untuk mengatasi masalah ini, Anda dapat menambahkan aturan firewall untuk mengizinkan akses ke port tersebut. Misalnya, jika Anda menggunakan `ufw`, Anda dapat menjalankan perintah berikut:

    ```bash
    sudo ufw allow 9870/tcp
    sudo ufw allow 8088/tcp
    ```

6.  Setelah menambahkan aturan firewall, pastikan untuk memuat ulang konfigurasi firewall:

    ```bash
     sudo ufw reload
    ```

7.  Setelah itu, coba akses kembali antarmuka web Hadoop di browser Anda.
8.  Jika masih mengalami masalah, anda dapat memeriksa port dengan perintah berikut:

    ```bash
    sudo netstat -tuln | grep 9870
    sudo netstat -tuln | grep 8088
    ```

    atau

    ```bash
    sudo netstat -tuln
    ```

9.  Pastikan bahwa port 9870 dan 8088 terbuka dan mendengarkan pada alamat IP yang benar. Jika tidak, periksa konfigurasi Hadoop Anda.
