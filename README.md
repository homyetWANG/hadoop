# hadoop学习笔记　Hadoop開発のメモ

基础知识　基礎知識
========

## 1.基础　基礎

hadoop `HDFS` 分布式文件系统 <br>
hadoop `MapReduce 2.0 YARN` <br>
[`Spark`](https://baike.baidu.com/item/SPARK/2229312?fr=aladdin "百度百科") <br>
机器学习 <br>


## 2/3. 系统和虚拟机的安装　システムと仮想マシンのインストール

Virtual box 下的虚拟机的安装 <br>
先安装虚拟机，然后在虚拟机中创建linux机器。之后下载ubuntu镜像iso，在linux机器的设置-存储中添加该镜像设备。启动linux。出现安装界面 <br>
在Linux虚拟机的选项卡-设备-安装增强功能中添加改变分辨率的应用 <br>
修改默认输入法（ubuntu设置-文本输入） <br>
在Linux虚拟机的选项卡-设备-共享剪贴板-双向 <br>


## 4.hadoop single node cluster的安装;　～のインストール

### 4.1)安装JDK <br>
java -version <br>
sudo apt-get update <br>
sudo apt-get install default-jdk <br>
java -version <br>
update-alternatives --display java <br>

### 4.2)設定 SSH 無密碼登入 <br>
sudo apt-get install ssh <br>
sudo apt-get install rsync <br> 
ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa <br> 
ll /home/hduser/.ssh <br>
ll ~/.ssh <br>
cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys <br>

### 4.3)下载安装hadoop <br>
wget http://ftp.twaren.net/Unix/Web/apache/hadoop/common/hadoop-2.6.0/hadoop-2.6.0.tar.gz  <br>
`wget`下载 <br>
sudo tar -zxvf Hadoop-2.6.0.tar.gz <br>
sudo tar -zxvf `Hadoop-3.0.0-alpha4.tar.gz` 本次下载、安装的这个<br>
`tar` 解压 <br>
sudo mv hadoop-2.6.0 /usr/local/hadoop <br>
`mv` move 移动到指定目录 <br>
 ll /usr/local/hadoop  <br>
 `ll`查看指定目录 <br>
 
 ### 4.4)设置环境变量 <br>
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

 ### 4.5)修改hadoop配置文件 <br>
#### 1修改hadoop-env.sh
sudo gedit /usr/local/hadoop/etc/hadoop/hadoop-env.sh <br>
輸入下列內容:
export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64 <br>
`export JAVA_HOME=/usr/lib/jvm/java-7-java-8-oracle` 本次安装的这个，所以写这个不写jdk7了<br>
#### 2修改core-site.xml
sudo gedit /usr/local/hadoop/etc/hadoop/core-site.xml <br>
`<configuration> <br>
<property> <br>
	<name>fs.default.name</name> <br>
	<value>hdfs://localhost:9000</value> <br>
</property> <br>
</configuration> <br>`

#### 3修改yarn-site.xml
sudo gedit /usr/local/hadoop/etc/hadoop/yarn-site.xml <br>
`<property> <br>
	<name>yarn.nodemanager.aux-services</name> <br>
	<value>mapreduce_shuffle</value> <br>
</property> <br>
<property> <br>
	<name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name> <br>
	<value>org.apache.hadoop.mapred.ShuffleHandler</value> <br>
</property> <br>`

#### 4修改mapred-site.xml
sudo gedit /usr/local/hadoop/etc/hadoop/mapred-site.xml
`<configuration> <br>
<property> <br>
	<name>mapreduce.framework.name</name> <br>
	<value>yarn</value> <br>
</property> <br>
</configuration> <br>`

#### 5修改hdfs-site.xml
sudo gedit /usr/local/hadoop/etc/hadoop/hdfs-site.xml <br>
`<property> <br>
	<name>dfs.replication</name> <br>
	<value>3</value> <br>
</property> <br>`
（设置blocks副本数量）
`<property> <br>
	<name>dfs.namenode.name.dir</name> <br>
	<value>file:/usr/local/hadoop/hadoop_data/hdfs/namenode</value> <br>
</property> <br>`
（设置NameNode数据存储目录）
`<property> <br>
	<name>dfs.datanode.data.dir</name> <br>
	<value>file:/usr/local/hadoop/hadoop_data/hdfs/datanode</value> <br>
</property> <br>`
（设置DataNode数据存储）

### 4.6)创建并格式化HDFS目录
sudo mkdir -p /usr/local/hadoop/hadoop_data/hdfs/namenode 创建NameNode数组存储目录 <br>
sudo mkdir -p /usr/local/hadoop/hadoop_data/hdfs/datanode 创建DataNode数组存储目录 <br>
sudo chown hduser:hduser -R /usr/local/hadoop `sudo chown hom:hom -R /usr/local/hadoop`将hadoop目录所有者变更hduser（[这里我运行时出现问题，没有用户然后创建了用户后面有不能格式化namenode，所以我就又改为hom了](http://blog.sina.com.cn/s/blog_6178edda0102x0cs.html "4.6问题/my blog")） <br>

hadoop namenode -format 将HDFS格式化<br>
* [hadoop创建前HDFS格式时出现的错误Unsupported major.minor version 52.0 ](http://blog.sina.com.cn/s/blog_6178edda0102x0cs.html "链接同上/my blog") <br>
* [Ubuntu安装JDK7/JDK8的两种方式 ](http://blog.sina.com.cn/s/blog_6178edda0102x0dh.html "my blog") 在出现版本问题时的解决方法<br>

### 4.7)启动hadoop
* `start-dfs.sh` 启动HDFS <br>
* `start-yarn.sh` 启动MapReduce YARN框架<br>
* `start-all.sh` 同时启动以上两个 <br>
[hadoop启动警告，运行start-dfs.sh时](http://blog.sina.com.cn/s/blog_6178edda0102x0er.html "my blog") 在尝试启动时遇到的情况，同时给出解决方法  <br>
* `jps` 查看进程是否启动 <br>

### 4.8)查看 Resource Manager和 NameNode HDFS 界面
打开火狐浏览器，地址栏输入http://localhost:8088/ Resource Manager界面 <br>
打开火狐浏览器，地址栏输入http://localhost:9870/ NameNode HDFS界面（[关于http://localhost:50070/地址打不开界面的问题](http://blog.sina.com.cn/s/blog_6178edda0102x0ex.html "my blog")可以查看这个） <br>



## 5.Hadoop Multi Node Cluster的安装

我们需要在建立几个虚拟机担任data node 的角色 <br>
如图 <br>
![](https://github.com/homyetWANG/hadoop/raw/master/5.png) <br>

### 5.1)在虚拟机中克隆clone hadoop
“hadoop”虚拟机右键克隆， 设置名称为“data1”，完全复制，完成 <br>
选择“data1”右键设置-网络-网卡1-NAT链接方式，同时选项卡-网卡2-勾选启用并选择host-only链接方式。确定 <br>

默认的的网卡IP段·192.168.56.X
所以准备设置如下： <br>
master：192.168.56.100 <br>
data1：192.168.56.101 <br>
data2：192.168.56.102 <br>
data3：192.168.56.103 <br>

### 5.3)
* 设置固定IP NAT网卡和 host-only网卡
sudo gedit /etc/network/interfaces  `编辑interfaces网络配置文件` <br>

\# NAT interface  <br>
auto eth0  <br>
iface eth0 inet dhcp  <br>

\# host only interface   <br>
auto eth1  <br>
iface eth1 inet static  <br>
address		192.168.56.101  <br>
netmask		255.255.255.0  <br>
network		192.168.56.0  <br>
broadcast	192.168.56.255  <br>

sudo gedit /etc/hostname
输入data1 入力

sudo gedit /etc/hosts
追加以下代码　次のコードをファイルに追加する
\# Ip address
192.168.56.100	master
192.168.56.101  data1
192.168.56.102  data2
192.168.56.103  data3

sudo gedit /usr/local/hadoop/etc/hadoop/core-site.xml
替换以下代码 <value>を次のように替える
<value>hdfs://master:9000</value>

sudo gedit /usr/local/hadoop/etc/hadoop/yarn-site.xml
<property>
	<name>yarn.resourcemanager.resource-tracker.address</name>
	<value>master:8025</value>
</property>
<property>
	<name>yarn.resourcemanager.scheduler.address</name>
	<value>master:8030</value>
</property>
<property>
	<name>yarn.resourcemanager.address</name>
	<value>master:8050</value>
</property>

![](https://github.com/homyetWANG/hadoop/raw/master/imageForMe/6.png "Apache Hadoop YARN") <br>
[Apache Hadoop YARN](http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) <br>

### 5.4)复制data1到data2，data3，master　data1をクローンしてdata2,data3,masterになる


### 5.5)
