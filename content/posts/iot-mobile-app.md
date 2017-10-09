---
layout: post
title: IoT and Mobile App
description: "A study case about a project that I did, using a mobile app to control an Arduino."
date: 2017-09-22
tags: [IoT App]
image:
  feature: /images/post-background.png
---
# The Project

My roommates are two exciting engineers, that love to create prototypes and small projects. Last winter, they built a coffee machine operated by an Arduino Board. Their idea was to create an automatic coffee machine, that you could configure the time for it to start. When the project was half done, they asked me if I could make an app that would trigger the coffee machine, because they wanted to do it while they are still on bed.

The app idea was super excited, so I decided to accept the challenge and develop something for them. The app project was also used for a course on my post graduation studies (Web and Mobile Development and Design in [Langara](https://langara.ca/)). So it was a win-win situation. In summary, the project scope was to create an App (for Android and iOS) capable to send a signal to the Arduino Board, that would process this call and trigger the coffee machine. We called it Smart Coffee.

# The Technologies

## Hardware
+ Arduino Mega
+ Wifi shield ESP8266

## Software
+ Visual Studio 2017
+ Xamarin
+ Android SDK 5.1

# The Solution

I am not going to focus on the Xamarin code because my intention here is to show how to integrate an App with an Arduino, regardless the framework.

Our first task was to configure ESP8266 with my local network. For that we used the following code, found at [LINK](https://github.com/esp8266/Arduino/blob/master/libraries/ESP8266WiFi/examples/WiFiClientBasic/WiFiClientBasic.ino)

```
/*
 *  This sketch sends a message to a TCP server
 *
 */

#include <ESP8266WiFi.h>
#include <ESP8266WiFiMulti.h>

ESP8266WiFiMulti WiFiMulti;

void setup() {
    Serial.begin(115200);
    delay(10);

    // We start by connecting to a WiFi network
    WiFiMulti.addAP("SSID", "passpasspass");

    Serial.println();
    Serial.println();
    Serial.print("Wait for WiFi... ");

    while(WiFiMulti.run() != WL_CONNECTED) {
        Serial.print(".");
        delay(500);
    }

    Serial.println("");
    Serial.println("WiFi connected");
    Serial.println("IP address: ");
    Serial.println(WiFi.localIP());

    delay(500);
}


void loop() {
    const uint16_t port = 80;
    const char * host = "192.168.1.1"; // ip or dns



    Serial.print("connecting to ");
    Serial.println(host);

    // Use WiFiClient class to create TCP connections
    WiFiClient client;

    if (!client.connect(host, port)) {
        Serial.println("connection failed");
        Serial.println("wait 5 sec...");
        delay(5000);
        return;
    }

    // This will send the request to the server
    client.print("Send this data to server");

    //read back one line from server
    String line = client.readStringUntil('\r');
    client.println(line);

    Serial.println("closing connection");
    client.stop();

    Serial.println("wait 5 sec...");
    delay(5000);
}

```
Once we stablished the connection, my roommates configured the pins so every time we had a connection, the arduino board would light up a LED. On this stage we had our connection working and the integration between Wifi Shield and Board working properly.

The next step was to create a loop statement on the Wifi Shield, in order to listen for requests. That was done by using the ESP8266 library, which contains the method recv().

```
uint8_t buffer[128] = {0};
uint8_t mux_id;
uint32_t len = wifi.recv(&mux_id, buffer, sizeof(buffer), 100);

if (len > 0) { //I got a request }

```
With that done, the last step was to create an event handler on the app to send a HTTP request to the WiFi Shield IP Address. On the request, I used a query string to send how many cups the user wants. On the Arduino side, it reads the query string and calls the appropriate code. Also, it sends a HTTP 200 response to the app, confirming the request' success.

# Conclusion

In conclusion, we created a loop to listen for any HTTP request, and once it captures one, it would process it and send a HTTP response. For the app side, it just needed to send a HTTP request to the WiFi Shield IP with the cups as query string.
