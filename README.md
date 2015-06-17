![](https://github.com/briankimstudio/wifi-weather-sensor/blob/master/esp8266-1.png)
#Introduction

Low-cost Wireless Weather Monitoring System

#Purpose

This system transmits temperature and humidity data to cloud on internet via Wifi and provides web interface to monitor those measured information.

In this system, ESP8266 reads data from DHT22 sensor, then, send it to data.sparkfun.com cloud system using simple HTTP protocol. For web interface, all charts are implemented with Google Chart APIs. 

#Prerequisites

- Arduino IDE
- ESP8266 package for Arduino IDE (*Refer to http://hpclab.blogspot.com/2015/06/esp8266-arduino-ide-on-mac-os-x.html for more information)

Compiling and uploading is done in Arduino IDE with ESP8266 package installed. Therefore, basic knowledge about Arduino IDE is required to proceed following steps.   

#Requirements
##Hardware

- ESP8266 ( Around $3 on ebay )
- DHT22 ( Around $7 on eBay )
- Resistor 10k(Pull up)

To keep the cost low, ESP8266 is used as not only controller, but also Wifi transmitter. DHT22 sensor can be replaced by any other temperature sensor. Pull up resistor is used for the input pin of ESP8266.
Software

- data.sparkfun.com
- Google Chart 

We used data.sparkfun.com cloud system to store measured data with two reasons. First, it provides free service and even does not require any account. We just need to create a 'free stream', which is basically a storage area. Secondly, it's easy to use. A single HTTP GET request can send measured data to cloud.

For web interface, Google Chart is used to draw gauge and graph to show historical change of temperature, humidity, and heat index.

#Instructions

##Setup hardware
###For uploading

Here is a wiring information to upload sketch to ESP8266 using USB to TTL converter. Since my converter is 5v version, I used another power supply to provide 3.3v to ESP8266. GPIO 0 should be pulled down to the ground for uploading.

BE CAREFUL! ESP8266 works with only 3.3v, not 5v!

![](https://github.com/briankimstudio/wifi-weather-sensor/blob/master/usb-to-ttl_bb.png)

###For running

Connect ESP8266 to DHT22 temperature sensor. GPIO 2 pin of ESP8266 is used to read measured data of DHT22.

![](https://github.com/briankimstudio/wifi-weather-sensor/blob/master/wifi-weather-sensor_bb.png)

##Step 1. Create 'free data stream' at data.sparkfun.com

Visit https://data.sparkfun.com/ and click 'CREATE' menu to create a 'free data stream'. It's like an identifier for your data to read and write. One interesting feature of data.sparkfun.com is that it doesn't require you to create or activate any account. Just create 'free data stream'. Just remember that no more than 100 requests within 15 minutes window are allowed. On average, one request is allowed every 9 seconds. Check here for detail.

![](https://github.com/briankimstudio/wifi-weather-sensor/blob/master/data.sparkfun1.png)

Among several fields in the creation form, 'Fields' and 'Stream Alias' are important. Fields are actual variable name for data when you send it with HTTP GET request. 'Stream Alias' is a name which can be used in URL to retrieve data from cloud.

![](https://github.com/briankimstudio/wifi-weather-sensor/blob/master/data.sparkfun2.png)

Once you finish creating 'stream', it provides several information such as private key, public key and simple instructions on how to send and retrieve data. Don't loose them, you need them to proceed to the next step.

##Step 2. Upload Sketch

For this step, you need those information at hand,

- SSID & Password of you Access Point
- List of field names you typed in 'Fields' in the creation form in Step 1
- Public Key from Step 1
- Private Key from Step 1

In this example, we used 3 fields to send data to cloud,

- temp: for temperature value measured in DHT22
- hum: for humidity value measured in DHT22
- hidx: for heat index calculated in the sketch

HTTP GET request for sending data looks like this,

data.sparkfun.com/input/YOUR_PUBLIC_KEY?private_key=YOUR_PRIVATE_KEY&temp=82.04&hum=53.10&hidx=83.29

Modify those values in the sketch according to your configuration. Then, compile and upload to ESP8266 using USB to TTL converter. If you face errors on Mac machine, refer to http://hpclab.blogspot.com/2015/06/esp8266-arduino-ide-on-mac-os-x.html for solution.

Once finish uploading, open 'Serial Monitor' in Arduino IDE, then you will see something like this,

![](https://github.com/briankimstudio/wifi-weather-sensor/blob/master/console.png)

To verify whether the data has been transmitted and stored in cloud without error, type this URL in web browser.

https://data.sparkfun.com/streams/YOUR_PUBLIC_KEY

You will see this kind of output if everything has gone well.

![](https://github.com/briankimstudio/wifi-weather-sensor/blob/master/data.sparkfun3.png)

##Step 3. Create HTML file

For this step, you need this information at hand,

- Public key from Step 1

Modify the value of public_key to your own key in example HTML file for web interface. Basically, it retrieves data in JSON format from cloud, then pass it to Google Chart API for drawing chart. In this example, gauge chart and line chart are used for demonstration.

#Results

Open HTML file with web browser, then you will see this chart right away. If you would like to access web interface from anywhere in the world, then, just upload this HTML file to your web site. It does not require any additional files or libraries or anything, just one single HTML file.  That's it!

![](https://github.com/briankimstudio/wifi-weather-sensor/blob/master/data.sparkfun4.png)
![](https://github.com/briankimstudio/wifi-weather-sensor/blob/master/esp8266-2.png)

#References

- https://data.sparkfun.com/
- http://phant.io/graphing/google/2014/07/07/graphing-data/
- http://hpclab.blogspot.com/2015/06/esp8266-arduino-ide-on-mac-os-x.html
- https://developers.google.com/chart/

Leave comments on [blog](http://hpclab.blogspot.com/2015/06/esp8266-based-wifi-weather-monitoring.html)
