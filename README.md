# webMqtt
Example of using mosquitto with websockets, and SmoothieCharts

```
cd /tmp
## Download and extract mosquitto
wget http://mosquitto.org/files/source/mosquitto-1.4.14.tar.gz
tar xzvf mosquitto-1.4.14.tar.gz
## Edit build configuration
cd mosquitto-1.4.14
vi config.mk
## turn on websockets: WITH_WEBSOCKETS:=yes 
## save and exit
## install libraries
sudo apt-get install libc-ares-dev uuid-dev libwebsockets-dev libssl-dev
## build 
make clean && make
mkdir /tmp/mosquitto

## install mosquitto
sudo make install
## -or-
cp client/mosquitto_[sp]ub /tmp/mosquitto
cp lib/libmosquitto.so.1 /tmp/mosquitto
cp src/mosquitto /tmp/mosquitto
## copy /tmp/mosquitto directory your preferred binaries location
##   e.g.: cp -r /tmp/mosquitto ~/bin
## you can then use mosquitto by:
##   LD_LIBRARY_PATH=~/bin/mosquitto ~/bin/mosquitto
## and likewise for mosquitto_sub and mosquitto_pub

mosquitto - the mqtt broker
mosquitto_pub - a command-line tool to publish data
mosquitto_pub - a command-line tool to subscribe to data

E.g. (in three terminals):
(1)> mosquitto
(2)> mosquitto_sub -t mytopic
(3)> mosquitto_pub -t mytopic -m "hi there!!"


## Example of regular posting messages
export LD_LIBRARY_PATH=~/bin/mosquitto
export PATH=$PATH:~/bin/mosquitto
count=0
while [ true ]
do 
  mosquitto_pub -t mytopic -m "$count"
  count=$((count + 1))
  count=$((count % 256))
  sleep 1
done


## Example of using mosquitto with websockets
## Install lighttpd
sudo apt-get install lighttpd
## Edit lighttpd.conf and set server.document-root to the ABSOLUTE path of
##   where the webmqtt.html, smoothie.js files are.
(1) lighttpd -f lighttpd.conf
(1) mosquitto
(2) Start regular posting example (see above)

In browser goto 'http://localhost:9002/webmqtt.html'.

You should now see the latest value in the box and see the data in the chart.
```







