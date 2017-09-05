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
sudo tar -zxvf hadoop-2.6.0.tar.gz <br>
`tar` 解压 <br>
sudo mv hadoop-2.6.0 /usr/local/hadoop <br>
`mv` move 移动到指定目录 <br>
 ll /usr/local/hadoop  <br>
 `ll`查看指定目录 <br>
 
 ### 4)设置环境变量 <br>
