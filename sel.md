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


Ref
--------
http://wiki.openqa.org/display/SRC/Selenium-RC+and+Continuous+Integration




