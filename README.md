# hadoop学习笔记

基础知识
========

## 1.
{
    hadoop HDFS 分布式文件系统;
    hadoop MapReduce 2.0 YARN;
    Spark;
    机器学习;
}

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
1)安装JDK;
java -version;
sudo apt-get update;
sudo apt-get install default-jdk;
java -version;
update-alternatives --display java;

2)設定 SSH 無密碼登入
sudo apt-get install ssh(不要分号);
sudo apt-get install rsync; 
ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa; 
ll /home/hduser/.ssh;
ll ~/.ssh;
cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys;

}
