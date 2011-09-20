---
title: Installations on OSX
layout: wikistyle
---

Mysql on Python
=====================


1. Get the tarball for Mysql-python from [http://sourceforge.net/projects/mysql-python/] (http://sourceforge.net/projects/mysql-python/)
1. In the untarred dir edit setup.cfg to change the location of mysql_cfg

        mysql_config = /usr/local/mysql/bin/mysql_config

2. Make the following symlinks (via [http://stackoverflow.com/questions/4730787/python-import-mysqldb-error-mac-10-6](http://stackoverflow.com/questions/4730787/python-import-mysqldb-error-mac-10-6)):

    sudo ln -s /usr/local/mysql/lib/libmysqlclient.18.dylib /usr/lib/libmysqlclient.18.dylib
    sudo ln -s /usr/local/mysql/lib /usr/local/mysql/lib/mysql

3. Run:

    python setup.py make
    sudo python setup.py install




