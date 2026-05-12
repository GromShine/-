#### 创建虚拟机

![image-20260314091949451](hadoopimg/image-20260314091949451.png)

![image-20260314092027322](hadoopimg/image-20260314092027322.png)

![image-20260314092114400](hadoopimg/image-20260314092114400.png)

![image-20260314092256398](hadoopimg/image-20260314092256398.png)

![image-20260314092420795](hadoopimg/image-20260314092420795.png)

![image-20260314092521810](hadoopimg/image-20260314092949581.png)

![image-20260314092631606](hadoopimg/image-20260314092631606.png)

![image-20260314092702306](hadoopimg/image-20260314092702306.png)

![image-20260314092742088](hadoopimg/image-20260314092742088.png)

![image-20260314092814563](hadoopimg/image-20260314092814563.png)

![image-20260314092911099](hadoopimg/image-20260314092911099.png)

![image-20260314092949581](hadoopimg/image-20260314092521810.png)

##### 配置完虚拟机以后就可以开机,安装系统![image-20260314093036487](hadoopimg/image-20260314093036487.png)

##### 鼠标点击虚拟机安装界面,进入到虚拟机,否则无法操作虚拟机,想要让鼠标从虚拟机出来,按键盘的ctrl+Alt组合键

![image-20260314093819411](C:\Users\admin\Desktop\hadoopimg/image-20260314093819411.png)

![image-20260314093955922](hadoopimg/image-20260314093955922.png)

1. 选择DATE & TIME 修改时区

   ![image-20260314094315802](hadoopimg/image-20260314094315802.png)

鼠标点击地图绿色位置,就会选择shanghai,然后点击Done

2. 找到INSTALLATION DESTINATION

![image-20260314094625465](C:\Users\admin\Desktop\hadoopimg/image-20260314094625465.png)

![image-20260314094815866](hadoopimg/image-20260314094815866.png)

![image-20260314094902820](hadoopimg/image-20260314094902820.png)

![image-20260314094957274](hadoopimg/image-20260314094957274.png)

![image-20260314095034556](hadoopimg/image-20260314095034556.png)

3. 点击 NETWORK & HOST NAME

   ![image-20260314095534902](hadoopimg/image-20260314095534902.png)

![image-20260314100857502](hadoopimg/image-20260314100857502.png)

![image-20260314105730993](hadoopimg/image-20260314105730993.png)

![image-20260314105847660](hadoopimg/image-20260314105847660.png)

![image-20260314110047066](hadoopimg/image-20260314110047066.png)

等待安装结束

![image-20260314110818897](hadoopimg/image-20260314110818897.png)

![image-20260314111035739](hadoopimg/image-20260314111035739.png)

输入用户名:root,回车

![image-20260314111130232](C:\Users\admin\Desktop\hadoopimg/image-20260314111130232.png)

然后再输入密码,注意,在这里输入密码,密码是不显示的,输入完密码后,回车,会进入系统

![image-20260314111250942](hadoopimg/image-20260314111250942.png)

如果出现如上信息,说明成功登录进了系统.接下来,就可以使用ssh客户端连接linux系统了

![image-20260314111506805](hadoopimg/image-20260314111506805.png)

![image-20260314111609311](hadoopimg/image-20260314111609311.png)

点击SSH后,会出现ssh的配置信息

![image-20260314111825207](hadoopimg/image-20260314111825207.png)

![image-20260314112202825](hadoopimg/image-20260314112202825.png)

![image-20260314112448747](hadoopimg/image-20260314112448747.png)

##### centos7 已经停止维护,需要先更换安装源

```shell
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
yum clean all
yum makecache
yum update
```

#### 配置hadoop

#### 关闭防火墙 关闭seLinux

> systemctl disable firewalld
>
> systemctl stop firewalld
>
> setenforce 0
>
> vi /etc/selinux/config
>
> 将SELINUX=enforcing改为SELINUX=disabled

#### 创建用户 hadoop并设置密码

> useradd -m hadoop -s /bin/bash
>
> passwd hadoop

#### 提升hadoop权限,不需要输入密码

> visudo
>
> 添加内容
>
> hadoop  ALL=(ALL)       NOPASSWD: ALL 

![image-20260314115610166](hadoopimg/image-20260314115610166.png)

#### 创建目录 用来存放软件,并把该文件夹设置为hadoop所有

> mkdir /opt/work -p
>
> chown hadoop.hadoop /opt/work

退出ssh客户端,更换hadoop用户登录系统

![image-20260314120343712](hadoopimg/image-20260314120343712.png)

![image-20260314120425591](hadoopimg/image-20260314120425591.png)

把需要用到的软件全部选中后,拖到左侧的目录中

![image-20260314120913283](hadoopimg/image-20260314120913283.png)

上传文件，并解压缩

```shell
# 把hadoop解压到/opt/work目录下
tar zxvf hadoop-3.3.6.tar.gz -C /opt/work
# 解压jdk
tar zxvf jdk-8u451-linux-x64.tar.gz -C /opt/work
# 解压spark
tar xvf spark-3.5.8-bin-hadoop3.tgz -C /opt/work
# 创建jdk的软连接
ln -s /opt/work/jdk1.8.0_451 /opt/work/jdk
# 创建hadoop的软连接
ln -s /opt/work/hadoop-3.3.6 /opt/work/hadoop
#创建spark的软连接
ln -s /opt/work/spark-3.5.8-bin-hadoop3 /opt/work/spark
```

安装Anaconda3

```shell
# 给文件添加权限,添加权限后,会看到文件变成绿色的
chmod u+x Anaconda3-2023.07-1-Linux-x86_64.sh
# 运行安装文件
sh ./Anaconda3-2023.07-1-Linux-x86_64.sh
# 在安装Anaconda3注意事项
1.在please,press Enter to continue位置处 #直接回车
2.在Do you accept the license terms?[yes|no] #这里也输入yes
[no] >>> yes  
3.在如下的位置处输入你的Anaconda3的位置,这里要输入 # /opt/work/anaconda3
Anaconda3 will now be installed into this location:
/home/hadoop/anaconda3
 - Press ENTER to confirm the location
 - press CTRL -C to abort the installation
 - Or specity a different location below
 [/home/hadoop/anaconda3] >>> /opt/work/anaconda3
 4.在by running conda init? [yes|no] #这里要输入yes
 [no] >>> yes
```

![image-20260314123001816](hadoopimg/image-20260314123001816.png)

最终/opt/work目录是这个样子

![image-20260314124052515](hadoopimg/image-20260314124052515.png)

配置python

```sh
# 创建软连接
sudo mv /usr/bin/python /usr/bin/python.bak
sudo ln -s /opt/work/anaconda3/bin/python3 /usr/bin/python3
sudo ln -s /opt/work/anaconda3/bin/python3 /usr/bin/python
```

#### 配置环境变量

sudo vi /etc/profile.d/env.sh

```sh
export JAVA_HOME=/opt/work/jdk
export HADOOP_HOME=/opt/work/hadoop
export SPARK_HOME=/opt/work/spark
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export ANACONDA_HOME=/opt/work/anaconda3
PATH=$PATH:$JAVA_HOME/bin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$ANACONDA_HOME/bin:$SPARK_HOME/bin
```

让环境变量生效

> source /etc/profile

#### Hadoop集群配置

注意:NameNode和SecondaryNameNode不要安装到同一台服务器

注意:ResourceManager很消耗内存,不要和NameNode.SecondaryNameNode配置在同一台机器上

|      | node1                  | node2                            | node3                           |
| ---- | ---------------------- | -------------------------------- | ------------------------------- |
| HDFS | NameNode<br />DataNode | <br />DataNode                   | SecondaryNameNode<br />DataNode |
| YARN | <br />NodeManager      | ResourceManager<br />NodeManager | <br />NodeManager               |

配置文件位置:/opt/work/hadoop/etc/hadoop

vi /opt/work/hadoop/etc/hadoop/core-site.xml

```xml
<configuration>
    <!-- 指定NameNode地址 -->
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://node1:9820</value>
    </property>
        <!-- 指定hadoop数据的存储目录 -->
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/opt/work/hadoop/data</value>
    </property>
    <!-- 开启垃圾桶功能 1440表示1440分钟，也就是24小时 -->
    <property>
        <name>fs.tash.interval</name>
        <value>1440</value>
    </property>
</configuration>
```

vi /opt/work/hadoop/etc/hadoop/hdfs-site.xml

```xml
<configuration>
    <!-- nn web端访问地址 -->
    <property>
        <name>dfs.namenode.http-address</name>
        <value>node1:9870</value>
    </property>
        <!-- 2nn web 端访问地址 -->
    <property>
        <name>dfs.namenode.secondary.http-address</name>
        <value>node3:9868</value>
    </property>
</configuration>
```

vi /opt/work/hadoop/etc/hadoop/mapred-site.xml

```xml
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    <!-- 历史服务器端地址 -->
    <property>
        <name>mapreduce.jobhistory.address</name>
        <value>node1:10020</value>
    </property>
    <!-- 历史服务器web端地址 -->
    <property>
        <name>mapreduce.jobhistory.webapp.address</name>
        <value>node1:19888</value>
    </property>
    <property>
        <name>yarn.app.mapreduce.am.env</name>
        <value>HADOOP_MAPRED_HOME=/opt/work/hadoop</value>
    </property>
    <property>
        <name>mapreduce.map.env</name>
        <value>HADOOP_MAPRED_HOME=/opt/work/hadoop</value>
    </property>
    <property>
        <name>mapreduce.reduce.env</name>
        <value>HADOOP_MAPRED_HOME=/opt/work/hadoop</value>
    </property>
</configuration>
```

vi /opt/work/hadoop/etc/hadoop/yarn-site.xml

```xml
<configuration>
    <!-- 指定MR走shuffle -->
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <!-- 指定ResourceManager的地址-->
    <property>
        <name>yarn.resourcemanager.hostname</name> 
        <value>node2</value>
    </property>
<!-- 关闭yarn对物理内存和虚拟内存的限制检查 -->
    <property>
        <name>yarn.nodemanager.pmem-check-enabled</name>
        <value>false</value>
    </property>
    <property>
        <name>yarn.nodemanager.vmem-check-enabled</name>
        <value>false</value>
    </property>
      <!-- 开启日志聚集功能 -->
  <property>
    <name>yarn.log-aggregation-enable</name>
    <value>true</value>
  </property>
  <!-- 设置日志聚集服务器地址 -->
  <property>  
    <name>yarn.log.server.url</name>  
    <value>http://node1:19888/jobhistory/logs</value>
  </property>
  <!-- 设置日志保留时间为7天 -->
  <property>
    <name>yarn.log-aggregation.retain-seconds</name>
    <value>604800</value>
  </property>
</configuration>
```

vi /opt/work/hadoop/etc/hadoop/workers

把原有的内容删掉,改成如下

```shell
node1
node2
node3
```



创建ip地址跟主机名直接的映射,这样以后就可以用主机名来访问服务器了,这里的node2,还有node3一会我们要通过克隆主机给复制出来,另外也要把对应的主机ip修改成对应的ip地址

sudo vi /etc/hosts

```sh
192.168.88.128 node1
192.168.88.129 node2
192.168.88.130 node3
```

![image-20260314133656503](hadoopimg/image-20260314133656503.png)



运行sudo shutdown -h 0 命令,关机,开始克隆虚拟机

![image-20260314125248994](hadoopimg/image-20260314125248994.png)

![image-20260314125404994](hadoopimg/image-20260314125404994.png)

![image-20260314125445368](hadoopimg/image-20260314125445368.png)

![image-20260314125541327](hadoopimg/image-20260314125541327.png)

克隆完毕后,注意:不要启动其他虚拟机,只选择刚刚克隆的node2启动,启动结束后,在虚拟机里输入root用户名及密码

然后输入命令: nmtui 来设置ip地址,以及主机名

![image-20260314130244873](hadoopimg/image-20260314130244873.png)

选择Edit a connection 后回车

![image-20260314130426740](hadoopimg/image-20260314130426740.png)

选择网卡后,回车

![image-20260314130540186](hadoopimg/image-20260314130540186.png)

输入一个不同的ip地址

![image-20260314131007729](hadoopimg/image-20260314131007729.png)

返回

![image-20260314131107179](hadoopimg/image-20260314131107179.png)

设置主机名



![image-20260314131209597](hadoopimg/image-20260314131209597.png)

![image-20260314131324237](hadoopimg/image-20260314131324237.png)

配置完后,输入命令,重启网络,让配置生效

> systemctl restart network

再选择node1(必须是关机状态),同理再克隆node3

![image-20260314131723427](hadoopimg/image-20260314131723427.png)

克隆完毕后,把node3启动起来,然后,在虚拟机中使用root登录,同理也要输入nmtui重新设置ip,以及主机名

![image-20260314132142236](hadoopimg/image-20260314132142236.png)

配置完后,输入命令,重启网络,让配置生效

> systemctl restart network

这时候,再把node1启动起来,至此,我们就有了3台虚拟机启动起来了,再用ssh客户端依此连接新的服务器,注意:需要用hadoop用户登录

![image-20260314132544505](hadoopimg/image-20260314132544505.png)

#### 创建密钥对 在node1,node2,node3分别执行如下命令:

> ssh-keygen -t rsa

![image-20260314133048090](hadoopimg/image-20260314133048090.png)

一路回车就可以把密钥对创建出来

#### 在node1，node2上将公钥授权给其他机器,注意:也要给自己复制一个,否则,后边使用自动启动hadoop时,没权限启动自己的服务

> ssh-copy-id node1
>
> ssh-copy-id node2
>
> ssh-copy-id node3

接下来对namenode所在节点node1进行格式化，其他节点无需手动格式化，后面再启动的时候无需再格式化

> hdfs namenode -format

![image-20260314135155577](hadoopimg/image-20260314135155577.png)

### 启动hadoop命令如下,也可以跳过,使用启动脚本启动(更方便直观)

启动hdfs

> node1服务器 命令： start-dfs.sh

启动YARN

> node2服务器命令：start-yarn.sh

启动历史服务器

> node1服务器命令：mapred --daemon start historyserver

##### 关闭顺序

> node1服务器 命令：stop-dfs.sh
>
> node2服务器命令：stop-yarn.sh
>
> node1服务器命令：mapred --daemon stop historyserver

#### 创建启动脚本

vi ~/myhadoop.sh

```shell
#!/bin/bash 
 
if [ $# -lt 1 ] 
then 
    echo "No Args Input..." 
    exit ; 
fi 
 
case $1 in 
"start") 
        echo " =================== 启动 hadoop 集群 ===================" 
 
        echo " --------------- 启动 hdfs ---------------" 
        ssh node1 "/opt/work/hadoop/sbin/start-dfs.sh" 
        echo " --------------- 启动 yarn ---------------" 
        ssh node2 "/opt/work/hadoop/sbin/start-yarn.sh" 
        echo " --------------- 启动 historyserver ---------------" 
        ssh node1 "/opt/work/hadoop/bin/mapred --daemon start historyserver" 
;; 
"stop") 
        echo " =================== 关闭 hadoop 集群 ===================" 
 
        echo " --------------- 关闭 historyserver ---------------" 
        ssh node1 "/opt/work/hadoop/bin/mapred --daemon stop historyserver" 
        echo " --------------- 关闭 yarn ---------------" 
        ssh node2 "/opt/work/hadoop/sbin/stop-yarn.sh" 
        echo " --------------- 关闭 hdfs ---------------" 
        ssh node1 "/opt/work/hadoop/sbin/stop-dfs.sh" 
;; 
*) 
    echo "Input Args Error..." 
;; 
esac 
```

chmod +x ~/myhadoop.sh

#### 查看三台服务器Java进程脚本执行权限

vi ~/jpsall.sh

```shell
#!/bin/bash 
 
for host in node1 node2 node3 
do 
        echo =============== $host =============== 
        ssh $host jps  
done
```

chmod +x ~/jpsall.sh

使用脚本启动hadoop服务

> ~/myhadoop.sh start

启动后使用jpsall脚本查看服务进程启动状态

> ~/jpsall.sh

![image-20260314135723366](C:\Users\admin\Desktop\hadoopimg/image-20260314135723366.png)

通过http://192.168.88.128:9870/ 可以访问hadoop

![image-20260314140429435](hadoopimg/image-20260314140429435.png)

#### 配置SparkOnYARN

cp /opt/work/spark/conf/spark-env.sh.template /opt/work/spark/conf/spark-env.sh

vi /opt/work/spark/conf/spark-env.sh

```sh
export JAVA_HOME=/opt/work/jdk
export HADOOP_HOME=/opt/work/hadoop
export HADOOP_CONF_DIR=/opt/work/hadoop/etc/hadoop
export YARN_CONF_DIR=/opt/work/hadoop/etc/hadoop
export SPARK_DIST_CLASSPATH=$(/opt/work/hadoop/bin/hadoop classpath)
SPARK_DAEMON_MEMORY=1g
SPARK_HISTORY_OPTS="-Dspark.history.fs.logDirectory=hdfs://node1:9820/spark/eventLogs/ -Dspark.history.fs.cleaner.enabled=true"
```

cp /opt/work/spark/conf/spark-defaults.conf.template /opt/work/spark/conf/spark-defaults.conf

vi /opt/work/spark/conf/spark-defaults.conf

```sh
spark.master               yarn
spark.eventLog.enabled     true
spark.eventLog.dir         hdfs://node1:9820/spark/eventLogs
spark.eventLog.compress    true
spark.yarn.historyServer.address  node1:18080
spark.yarn.jars    hdfs://node1:9820/spark/jars/*
```

cp /opt/work/spark/conf/log4j2.properties.template  /opt/work/spark/conf/log4j2.properties

vi /opt/work/spark/conf/log4j2.properties

```shell
#大约在19行,修改为
rootLogger.level = warn
```

在hadoop中创建文件夹/spark/jars/,并把spark对应的jar包上传到/spark/jars/下

```shell
hadoop fs -mkdir -p /spark/eventLogs
hadoop fs -mkdir -p /spark/jars
hadoop fs -put $SPARK_HOME/jars/* /spark/jars/
```

##### 测试 官方圆周率

> spark-submit --master yarn /opt/work/spark/examples/src/main/python/pi.py 10

提交自己开发的程序

```shell
# 指定运行资源
spark-submit \
--master yarn \
--deploy-mode client \
--driver-memory 1G \
--driver-cores 1  \
--supervise \
--executor-memory 1G \
--executor-cores 2 \
--num-executors 2 \
/opt/work/spark/examples/src/main/python/pi.py 10
```

计算方式：资源的70%运行spark程序

> driver-cores  一般1个核就够了
>
> deploy-mode  client(Driver运行在客户端,AppMaster与Driver共存)，谁提交在谁那里执行（调试用）cluster （Driver运行在Node Manager，Driver以子进程方式运行在AppMaster进程内部）（正式使用）
>
> driver-memory  小型的1G，返回数据小于100M给2-4G，小于500M给4-8G
>
> supervise 守护，driver崩溃后重启driver
>
> executor-cores 比较建议给2个核心，太多也不好
>
> --num-executors 开启的executor数量，要根据你的可用内存以及cpu核心数计算 * num-executors就是实际使用的资源

# 只需要在node1上安装pyspark

可以本地安装

>  pip install pyspark-3.5.8-py2.py3-none-any.whl

也可以在线安装

>  pip install pyspark==3.5.8 -i https://pypi.tuna.tsinghua.edu.cn/simple/

pyCharm创建一个简单的项目

![image-20260129160506906](hadoopimg/image-20260129160506906.png)

![image-20260129162734491](hadoopimg/image-20260129162734491.png)

![image-20260129153630484](hadoopimg/image-20260129153630484.png)



![image-20260129153556985](hadoopimg/image-20260129153556985.png)

![image-20260129153920414](hadoopimg/image-20260129153920414.png)

![image-20260129154210361](hadoopimg/image-20260129154210361.png)

![image-20260129154329580](hadoopimg/image-20260129154329580.png)

```shell
# 在Linux上创建用于存放代码的目录：
mkdir -p /home/hadoop/pyspark_code
```

![image-20260129162004929](hadoopimg/image-20260129162004929.png)

![image-20260129161904189](hadoopimg/image-20260129161904189.png)

#### 注意：如果pycharm 连接远程服务器出现failed to prepare the environment.

> 修改linux服务器如下ssh的如下两个配置：
>
> sudo vi /etc/ssh/sshd_config
>
> AllowTcpForwarding yes
>
> GatewayPorts yes
>
> sudo systemctl restart sshd
>
> 修改完以后，清理缓存
>
> file- Invalidate Caches 全部选中后，Invalidate and Restart

#### spark开发，可以做成模板

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

from pyspark import SparkContext, SparkConf
import re

if __name__ == '__main__':
    conf = SparkConf().setMaster("local[2]").setAppName("wfit")
    sc = SparkContext(conf=conf)
    input_rdd = sc.textFile(name="hdfs:///spark/wordcount/input/word.txt")

    rs_rdd = (input_rdd
              # 去掉文件中空行,strip字符串函数，用于去首尾空格
              .filter(lambda line: len(line.strip()) > 0)
              # 将一行多个单词转换为一行一个单词
              .flatMap(lambda line: re.split(r"\s+", line.strip()))
              # 将每个元素构建成二元组，（word, 1）
              .map(lambda word: (word, 1))
              # 按照Key分组，将相同Key的Value放入一个列表中，并聚合 => KV结果（单词，次数）
              .reduceByKey(lambda tmp, item: tmp + item)
              # 按照单词出现的次数降序排序
              .sortBy(keyfunc=lambda tuple: -tuple[1])
              )

    # step3：保存结果：输出或者保存RDD的结果数据
    print(rs_rdd.collect())
    # 保存为文件
    # rs_rdd.saveAsTextFile(path="/spark/wordcount/out/wc-output889")
    sc.stop()
```

