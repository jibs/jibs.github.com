---
title: Hadoop admin wiki
layout: wikistyle
---


Hadoop - running a Hadoop cluster
================================

LZO Compression
================

Why you need this: Gzip files are not indexable, which means (indirectly) that only a single mapper can read them at any one time. This becomes a bottleneck for large file use cases. In general you want compression because MR jobs on Hadoop tend to be disk IO bound.


* Grab the LZO bins from here: https://github.com/kevinweil/hadoop-lzo
* This is an improved version of the libs available here: http://code.google.com/a/apache-extras.org/p/hadoop-gpl-compression/wiki/FAQ?redir=1 - but the build instructions remain the same
* The 32-bit instructions are not relevant, so all you need is:

    export JAVA_HOME=/usr/lib/jvm/java-6-sun
    export CFLAGS=-m64
    export CXXFLAGS=-m64
    ant compile-native tar

Gotchas:
* Make sure you have the JDK (and not just the JRE)!

For Cloudera distribution of hadoop, the second command looks like:
    
    mkdir /usr/lib/hadoop/lib/native
    sudo cp hadoop-lzo-0.4.14.jar /usr/lib/hadoop/lib/
    tar -cBf - -C /home/ubuntu/libs/hadoop-lzo/build/native/Linux-amd64-64 . | tar -xBvf - -C /usr/lib/hadoop/lib/native/

or for 32 bit machines:

    mkdir /usr/lib/hadoop/lib/native
    sudo cp hadoop-lzo-0.4.14.jar /usr/lib/hadoop/lib/
    tar -cBf - -C /home/ubuntu/libs/hadoop-lzo/build/native/Linux-i386-32 . | tar -xBvf - -C /usr/lib/hadoop/lib/native/

To test that it works:

    cd /tmp
    echo "hello world" > test.log
    lzop test.log
    hadoop fs -copyFromLocal test.log.lzo /tmp
    hadoop jar /usr/lib/hadoop/lib/hadoop-lzo-0.4.13.jar com.hadoop.compression.lzo.LzoIndexer /tmp/test.log.lzo


in /home/ubuntu/conf/hadoop-env.sh:

    sudo vim /usr/lib/hadoop/conf/hadoop-env.sh

    export JAVA_LIBRARY_PATH=$JAVA_LIBRARY_PATH:/usr/lib/hadoop-0.20/lib/native/Linux-amd64-64:/usr/lib/hadoop-0.20/lib/native:/usr/lib/hadoop/lib/native/lib

For 32 bit

    export JAVA_LIBRARY_PATH=$JAVA_LIBRARY_PATH:/usr/lib/hadoop-0.20/lib/native/Linux-i386-32:/usr/lib/hadoop-0.20/lib/native:/usr/lib/hadoop/lib/native/lib


in .bashrc:



    #export HADOOP_INSTALL=/usr/lib/
    #export HADOOP_HOME=/usr/lib/hadoop
    export JAVA_LIBRARY_PATH=/usr/lib/hadoop/lib/native/lib:/usr/lib/hadoop/lib:$JAVA_LIBRARY_PATH
    export PATH=/usr/lib/hadoop/lib/native:/usr/lib/hadoop/lib:/usr/lib/hadoop/lib/native/lib:$PATH
    #export HADOOP_CLASSPATH=/home/ubuntu/libs/hadoop-lzo/build/hadoop-lzo-0.4.13.jar
    

Make sure the path points to the folder with the actual source file (.so), as these are the JNI native headers that the code needs to find for compression - in the standard build these were located in `/home/ubuntu/libs/hadoop-lzo/build/native/Linux-amd64-64/lib`

If you have built it correctly, the folder should look like:

    ubuntu@ip-XXXXX:~/libs/hadoop-lzo/build/native/Linux-amd64-64/lib$ ls
    libgplcompression.a   libgplcompression.so    libgplcompression.so.0.0.0
    libgplcompression.la  libgplcompression.so.0


Testing
----------

To test a barebones indexing of the lzo compressor:

    cd /tmp
    echo "hello world" > test.log
    lzop test.log
    hadoop fs -copyFromLocal test.log.lzo /tmp
    hadoop jar /usr/lib/hadoop/lib/hadoop-lzo-0.4.14.jar com.hadoop.compression.lzo.LzoIndexer /tmp/test.log.lzo


Using LZO indexing in hive:

    CREATE EXTERNAL TABLE foo (
             columnA string,
             columnB string )
        PARTITIONED BY (date string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY "\t"
        STORED AS INPUTFORMAT "com.hadoop.mapred.DeprecatedLzoTextInputFormat"
              OUTPUTFORMAT "org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat"
        LOCATION '/path/to/hive/tables/foo';




Scheduler
=============

Setting up fair scheduler
    http://hadoop.apache.org/common/docs/r0.20.2/fair_scheduler.html

A helpful prez
    http://www.cs.berkeley.edu/~matei/talks/2009/hadoop_summit_fair_scheduler.pdf

Make sure the pools are added (for ex. in pools.xml)


Connecting to hive
------------------

We can add these props to the `hive/conf/hive-site.xml`

    <property>
    <name>mapred.fairscheduler.poolnameproperty</name>
    <value>pool.name</value>
    </property>

    <property>
    <name>pool.name</name>
    <value>${user.name}</value>
    </property>

Then all jobs will run as pool == the unix username (make sure these
pools are created)



Hadoop pricing on EC2
========================

    Memory(GB)	Compute units	Storage(GB)		Price	Price per compute unit
    Large	7.5	4	850		0.38	0.095
    Extra large	15	8	1690		0.76	0.095
    High-CPU Medium*	1.7	5	350		0.19	0.038
    High-CPU Extra Large	7	20	1690		0.76	0.038

*32 bit