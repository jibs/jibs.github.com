---
title: Selenium
layout: wikistyle
---

### Installing Selenium on Ubuntu

Download the full selenium standalone jar server from:
  http://code.google.com/p/selenium/downloads/detail?name=selenium-server-standalone-2.0rc2.jar&can=2&q=

Get the python bindings from
  sudo pip install selenium

Install firefox with
  sudo apt-get install firefox

Install startx etc:

http://blog.martin-lyness.com/archives/installing-xvfb-on-ubuntu-9-10-karmic-koala


startx -- `which Xvfb` :1 -screen 0 1024x768x24 2>&1 >/dev/null &

run with:
DISPLAY=:1 java -jar selenium-server.jar


Installing browsers
====================

Google chrome
--------------
Download the latest deb package from Goog

wget https://dl-ssl.google.com/linux/direct/google-chrome-stable_current_amd64.deb

sudo dpkg -i google-chrome-stable_current_amd64.deb 
sudo apt-get -f install

then re-run:

sudo dpkg -i google-chrome-stable_current_amd64.deb 



Update
-------

This was working, but in the end windows offers a much wider array of
browser problem cases (at least its good for something), particularly
the ie family, where most issues are likely to occur in any case.


Ref
--------
http://wiki.openqa.org/display/SRC/Selenium-RC+and+Continuous+Integration




