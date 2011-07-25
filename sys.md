---
title: Sysadmin snippets for high performance webservers
layout: wikistyle
---

Sysadmin stuff
===================


Increase file limits
----------------------
You need this if you have loads to concurrent connections etc. on node
(or even gevent / eventlet etc). 

So:
pam limits by default is not loaded in ubuntu. So run the following command in a terminal
 
    sudo vim /etc/pam.d/su
 
then uncomment the following line
 
    #session    required   pam_limits.so
 
save and close the editor.


sudo vim /etc/security/limits.conf
 
and add the following lines to the end of the file (before the line # End of file)
    *               soft    nofile            4084
    *               hard    nofile            4084

Source:
http://www.ubun2.com/question/433/how_set_ulimit_ubuntu_linux_getting_sudo_ulimit_command_not_found_error


