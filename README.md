# hadoop学习笔记

基础知识
========

## 1.

hadoop `HDFS` 分布式文件系统 <br>
hadoop `MapReduce 2.0 YARN` <br>
[`Spark`](https://baike.baidu.com/item/SPARK/2229312?fr=aladdin "百度百科") <br>
机器学习 <br>


## 2/3. 系统和虚拟机的安装;
{
Virtual box 下的虚拟机的安装;
先安装虚拟机，然后在虚拟机中创建linux机器。之后下载ubuntu镜像iso，在linux机器的设置-存储中添加该镜像设备。启动linux。出现安装界面;
在Linux虚拟机的选项卡-设备-安装增强功能中添加改变分辨率的应用;
修改默认输入法（ubuntu设置-文本输入）;
在Linux虚拟机的选项卡-设备-共享剪贴板-双向;
}

## 4.hadoop single node cluster的安装;
{
### 1)安装JDK <br>
java -version <br>
sudo apt-get update <br>
sudo apt-get install default-jdk <br>
java -version <br>
update-alternatives --display java <br>

### 2)設定 SSH 無密碼登入 <br>
sudo apt-get install ssh <br>
sudo apt-get install rsync <br> 
ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa <br> 
ll /home/hduser/.ssh <br>
ll ~/.ssh <br>
cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys <br>

### 3)下载安装hadoop <br>
wget http://ftp.twaren.net/Unix/Web/apache/hadoop/common/hadoop-2.6.0/hadoop-2.6.0.tar.gz  <br>
`wget`下载 <br>
sudo tar -zxvf Hadoop-2.6.0.tar.gz <br>
sudo tar -zxvf `Hadoop-3.0.0-alpha4.tar.gz` 本次下载、安装的这个<br>
`tar` 解压 <br>
sudo mv hadoop-2.6.0 /usr/local/hadoop <br>
`mv` move 移动到指定目录 <br>
 ll /usr/local/hadoop  <br>
 `ll`查看指定目录 <br>
 
 ### 4)设置环境变量 <br>
 sudo gedit ~/.bashrc <br>
编辑  该文件 <br>
>> export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64 <br>
`export JAVA_HOME=/usr/lib/jvm/java-8-oracle`本次安装的这个，所以写这个不写jdk7了 <br>
>> export HADOOP_HOME=/usr/local/hadoop  <br>
>> export PATH=$PATH:$HADOOP_HOME/bin  <br>
>> export PATH=$PATH:$HADOOP_HOME/sbin  <br>
>> export HADOOP_MAPRED_HOME=$HADOOP_HOME  <br>
>> export HADOOP_COMMON_HOME=$HADOOP_HOME  <br>
>> export HADOOP_HDFS_HOME=$HADOOP_HOME  <br>
>> export YARN_HOME=$HADOOP_HOME  <br>
>> export HADOOP_COMMON_HOME=$HADOOP_HOME  <br>
>> export HADOOP_HDFS_HOME=$HADOOP_HOME  <br>
>> export YARN_HOME=$HADOOP_HOME  <br>
>> export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib"  <br>
>> export JAVA_LIBRARY_PATH=$HADOOP_HOME/lib/native:$JAVA_LIBRARY_PATH

source ~/.bashrc <br>
使之生效 <br>

 ### 5)修改hadoop配置文件 <br>
#### 修改hadoop-env.sh
sudo gedit /usr/local/hadoop/etc/hadoop/hadoop-env.sh <br>
輸入下列內容:
export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64 <br>
`export JAVA_HOME=/usr/lib/jvm/java-7-java-8-oracle` 本次安装的这个，所以写这个不写jdk7了<br>
#### 修改core-site.xml
sudo gedit /usr/local/hadoop/etc/hadoop/core-site.xml <br>
`<configuration>
<property>
	<name>fs.default.name</name>
	<value>hdfs://localhost:9000</value>
</property>
</configuration>`

Step3 修改yarn-site.xml <br>
sudo gedit /usr/local/hadoop/etc/hadoop/yarn-site.xml <br>
在之間，輸入下列內容:
yarn.nodemanager.aux-services mapreduce_shuffle yarn.nodemanager.aux-services.mapreduce.shuffle.class org.apache.hadoop.mapred.ShuffleHandler <br>
Step4 修改mapred-site.xml
sudo cp /usr/local/hadoop/etc/hadoop/mapred-site.xml.template /usr/local/hadoop/etc/hadoop/mapred-site.xml sudo gedit /usr/local/hadoop/etc/hadoop/mapred-site.xml
在之間，輸入下列內容:
mapreduce.framework.name yarn
Step5 修改hdfs-site.xml
sudo gedit /usr/local/hadoop/etc/hadoop/hdfs-site.xml
在之間，輸入下列內容:
   dfs.replication    3    dfs.namenode.name.dir    file:/usr/local/hadoop/hadoop_data/hdfs/namenode    dfs.datanode.data.dir    file:/usr/local/hadoop/hadoop_data/hdfs/datanode  
 
