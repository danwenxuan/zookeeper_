# zookeeper_
集群部署zookeeper
-----------------------------------------------------------------------------

### 机器部署
- 安装到3台虚拟机上
- 安装好JDK

### 上传用工具。
- 解压
- su – hadoop（切换到hadoop用户）
- tar -zxvf zookeeper-3.4.5.tar.gz（解压）

### 重命名
- mv zookeeper-3.4.5 zookeeper（重命名文件夹zookeeper-3.4.5为zookeeper）

### 修改环境变量
- su – root(切换用户到root)
- vi /etc/profile(修改文件)
- 添加内容：
- export ZOOKEEPER_HOME=/home/hadoop/zookeeper
- export PATH=$PATH:$ZOOKEEPER_HOME/bin

### 重新编译文件：
- source /etc/profile
- 注意：3台zookeeper都需要修改
- 修改完成后切换回hadoop用户：
- su - hadoop

### 修改配置文件
- 用hadoop用户操作
- cd zookeeper/conf
- cp zoo_sample.cfg zoo.cfg
- vi zoo.cfg

### 添加内容：
- dataDir=/home/hadoop/zookeeper/data
- dataLogDir=/home/hadoop/zookeeper/log
- server.1=slave1:2888:3888 (主机名, 心跳端口、数据端口)
- server.2=slave2:2888:3888
- server.3=slave3:2888:3888

### 创建文件夹：
- cd /home/hadoop/zookeeper/
- mkdir -m 755 data
- mkdir -m 755 log

### 在data文件夹下新建myid文件，myid的文件内容为：
- cd data
- vi myid
添加内容：

### 集群下发到其他机器上
- scp -r /home/hadoop/zookeeper hadoop@slave2:/home/hadoop/
- scp -r /home/hadoop/zookeeper hadoop@slave3:/home/hadoop/

### 修改其他机器的配置文件
- 到slave2上：修改myid为：2
- 到slave3上：修改myid为：3

### 启动（每台机器）
- zkServer.sh start
