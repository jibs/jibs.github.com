Thoughts / rants on Hadoop
=============================

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
