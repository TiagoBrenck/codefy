---
layout: post
title: AWS IoT
description: "An explanation of AWS IoT service and some tutorials."
date: 2017-10-10
tags: [IoT App]
image:
  feature: /images/post-background.png
---

## IoT
IoT stands for Internet of Things, and it is  basically a network of physical devices that share data and communicate with each other.

Some IoT examples are:

 - Automated Home
 - Smart cities (city services exchanging data to take decision)
 - Smart watches
 - Gardening sensors
 - Heart beat tracker

## AWS IoT
“AWS IoT is a managed cloud platform that lets connected devices - cars, light bulbs, sensor grids, and more - easily and securely interact with cloud applications and other devices.” (Amazon.com description). This service allows you to connect, store states, and manage the devices on the cloud. It can also process data, filter them, transform and act upon data from the devices on the fly. In other words, it provides you services where you can create your own code that can trigger others AWS services.

### Communication
The communication can be done via four types of protocols:

| Protocol  |  Authentication |  Port  |
|:-----------|:-----------------|:--------|
|  MQQT     | Client Certificate  | 8883  |
|  HTTP     | Client Certificate  | 8443  |
|  HTTP     |  SigV4 |  443 |
|  MQTT + WebSocket | SigV4  | 443  |

### Rules
When you are creating your AWS IoT service, you have the option to setup what they call Rules. Rules give your devices the ability to interact with AWS services. You can use rules to support tasks like:

 - Save a file to Amazon S3.
 - Invoke a Lambda function to extract data.
 - Send a push notification to all users using Amazon SNS.
 - Write data received from a device to an Amazon DynamoDB database.

### Device Shadows
A thing shadow is a JSON document that is used to store and retrieve current state information for a thing (device, app, and so on). You can use thing shadows to get and set the state of a thing over MQTT or HTTP, regardless of whether the thing is connected to the Internet. Each thing shadow is uniquely identified by its name. With that, your physical devices won't miss any update in case they get disconnected.

### Quick Tutorial
Creating your thing on AWS IoT is easy.

1. First log in your AWS account ([AWS Link](aws.amazon.com)) and select **AWS IoT**.
2. On the right sidebar, click on **Registry > Register a thing**.
3. Type the name of your thing (for example "MyFirstThing") and click on **Create Thing**.
4. Amazon will redirect you to a screen where you can see your created thing inside a box. This means that your thing is created, however it needs the certifications for the communication between you app and AWS IoT. So, for that, **click on that box**.
5. Under **Security** tab, click on **Create certificate**.
6. Just by clicking the button, the certifications get created and you have the links to download it. On the same screen, click on the **Attach a policy** button.
7. Policies are like permissions for that certificate. You can have a certificate with admin privileges and another with just read privileges. For the sake of this example, we are going to create an admin one. Click on **Create new policy**.
8. Name it Admin and inside the Action textbox, type **\***. For the Effect option, click **Allow** (we are allowing the action to be executed), then click on **Create** button.
9. Now, click on **Security > Certificates**. Select your certificate and click on **Action** located on the bottom right of the black box containing your certificate ID. Then, click on **Attach policy**, select the Admin policy that we have just created and click **Attach**.
10. Click on **Action** again, but this time select **Attach a thing**. Then select our thing, MyFirstThing, and click **Attach**.
11. By this step, you have your thing created with its certificates and policies attached. If you click on **Registry > Things > MyFirstThing > Interact**, Amazon has provided you the REST API Endpoint for you interact with your thing.

### ESP8266 and AWS
In order to connect your AWS IoT with ESP8266 module (Arduino wifi shield) you can use these two libraries and it setup according to this code.

```
#include <AmazonIOTClient.h>
#include <ESP8266AWSImplementations.h>

void initAWS()
{
  iotClient.setAWSRegion("eu-west-1");
  iotClient.setAWSEndpoint("amazonaws.com");
  iotClient.setAWSDomain("xxx.amazonaws.com");
  iotClient.setAWSPath("/things/XXX/shadow");
  iotClient.setAWSKeyID("XXX");
  iotClient.setAWSSecretKey("YYY");
  iotClient.setHttpClient(&httpClient);
}

```

You can find a demo at: https://github.com/CheapskateProjects/ESP8266AWSIoTDemo/blob/master/ESP8266AWSIoTDemo.ino
