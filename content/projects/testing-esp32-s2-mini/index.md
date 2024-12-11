+++
date = '2024-12-08T21:34:15Z'
draft = true
title = "Testing Esp32-S2 Minis"
tags = ["projects"]
showComments = true
+++

## ESP32 WLED project

I wanted a to try some new hardware for running WLED projects (mostly for making signs and other CNC projects). I asked around on social media and had a couple of reccomendations. I received several recomendations:

* [ESP32-S2](https://www.amazon.com/gp/product/B0CKLGGNKY/)
* [ESP-WROOM-32 Development Board Programmer](https://www.amazon.com/dp/B0CFZJ4XCT?social_share=cm_sw_r_ud_dp_6P14R6WZHASS667TTXZW&peakEvent=4&dealEvent=0&language=en-US&skipTwisterOG=1&th=1)
    * [ESP32 Modules](https://www.mouser.com/ProductDetail/Espressif-Systems/ESP32-S3-WROOM-1-N16R8?qs=Li%252BoUPsLEnvQc9gW6AMhZg%3D%3D)

I took the dive, bought both of those solutions, currently set the programmer on the back burner, but will have more updates on that one later.

### The ESP32-S2 option

Spent some time today looking into flashing these things with WLED, My first search led me to this post on reddit:

[Any possibility to install WLED on ESP32-S2 Mini board?](https://www.reddit.com/r/WLED/comments/11ww2zo/any_possibility_to_install_wled_on_esp32s2_mini/)

So it led me down a sucessful path with the following steps. (There might be better ways, this was just the first thing I've tried)
* Installed the latest Tasmoto firmware Here [tasmoto](https://tasmota.github.io/install/)
    * Plugged in the board into my computer using USBC
        * Had to hit the Reset and '0' buttons a few times to get my Windows 10 box to recognize it. 
* Set the new tasmodo device to use local network
    * Used my Android phone to connect to the tasmodo WAP that showed up
    * Set the network setting to use my local WAP
* Update firmware
    * Browsed to the device on my local network
    * Selected "Update Firmware" 
    * Used the URL for the latest C2 [latest firmware] (https://github.com/Aircoookie/WLED/releases/download/v0.14.4/WLED_0.14.4_ESP32-S2.bin)
* Setup WLED
    * Browsed to the WLED page using the IP address
    * Entered the settings to connect to my local network

