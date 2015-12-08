---
layout: default
title: install spark&hadoop on mac 
---

## {{ page.title }}
* brew install hadoop<br/>
* brew install apache-spark scala sbt<br/>
vim ~/.bash_profile
export JAVA_HOME=$(/usr/libexec/java_home)
export HADOOP_HOME=/usr/local/homebrew/Cellar/hadoop/2.7.1
export HADOOP_CONF_DIR=$HADOOP_HOME/libexec/etc/hadoop
export SCALA_HOME=/usr/local/homebrew/Cellar/apache-spark/1.4.1

 set path variables
export PATH=$PATH:$HADOOP_HOME/bin:$SCALA_HOME/bin
. ~/.bash_profile
ssh 登陆
在mac 下要开启remote logig
ssh-keygen -t rsa cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys 
第一步 修改Hadoop的配置
cd  /usr/local/homebrew/Cellar/hadoop/2.7.1
vim etc/hadoop/core-site.xml
<property>
    <name>fs.defaultFS</name>
    <value>hdfs://localhost:9000</value>
  </property>
 <property>
        <name>hadoop.tmp.dir</name>
        <value>file:/usr/local/hadoop/tmp</value>
        <description>Abase for other temporary directories.</description>
    </property>
vim etc/hadoop/hdfs-site.xml
<property>
    <name>dfs.replication</name>
    <value>1</value>
  </property>
<property>
        <name>dfs.namenode.name.dir</name>
        <value>file:/usr/local/hadoop/tmp/dfs/name</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:/usr/local/hadoop/tmp/dfs/data</value>
    </property>
vim etc/hadoop/mapred-site.xml
<property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
  </property>
vim etc/hadoop/yarn-site.xml
     <property>
          <name>yarn.nodemanager.aux-services</name>
              <value>mapreduce_shuffle</value>
             </property>    
第二步 
cd /usr/local/homebrew/Cellar/hadoop/2.7.1 

./bin/hdfs namenode -format
./sbin/start-dfs.sh

./sbin/start-yarn.sh

jps
http://localhost:8088/

./bin/hdfs dfs -mkdir -p /user/geda
./bin/hadoop jar libexec/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.1.jar pi 10 100 
第三步 测试spark
cd /usr/local/homebrew/Cellar/apache-spark/1.4.1
./bin/run-example SparkPi
测试spark-shell
