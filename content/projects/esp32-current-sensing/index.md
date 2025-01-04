
+++
date = '2024-12-08T21:34:15Z'
draft = false
title = "Current Sensing with Esp32"
tags = ["projects"]
showComments = true
+++

## Project description

Attempting to use the esp32 to sense when my table saw is on and using homeassistant to turn on and off my dust collector.

I first attempted to use off the shelf shelly switches, but the current draw was too high. I would burn them out.

## Log

* 12-15-2024
   * Tested circuit - I can see the change of the voltage on the GPIO1 pin, seems to go down when current is applied
   * Looking into MQTT for homassistant integration, see video in web references
   * **Would like to use off-the-shelf tasmoto to simplify setup**
* 12-29-2024
   * Got it working by kind of buteforcing myself around the problem.
    * Set GPIO1 to be ADC input
    * ran command in the tasmodo console: `AdcParam 1,0,743,0.121,1.5`
    * got most of the values from [here](https://tasmota.github.io/docs/ADC/) but adjusted the 3rd and 4th and 5th params.
        * Arrived at the  values by adjusting the 4th to work fior 120 volts
        * For the third, I pluggin in an electric kettle and got the real values from an inline power meter like [this](https://amzn.to/4fFOy6m) Once I had them I adjusted the third param until the numbers on the tasmodo web page were approxmatly the same.
        * Set the last param to where It would read as 0 when the kettle was off, but would still detect when on. Might need adjusted when deployed.
* 01-01-2025
    * Tested [Mini Step-Down Power Supply Module](https://amzn.to/40bDZDL)
        * turns out I need to power the s2 mini from the vbus bin and I need 5v, so no need to split the voltage
        * Used heat shrink tubes and [WAGO 221 Lever Nuts](https://amzn.to/4a7PCPy) to attempt to keep it safe.
        * Acknowledge that I need to use fuses and such to make it safer "in production" - [reference](https://randomnerdtutorials.com/esp8266-hi-link-hlk-pm03/)


## Possible Products

1. [AC Current Sensor Split-Core Clamp Current Transformer](https://amzn.to/4hdxpCz)
2. [HiLetgo 3pcs ZMCT103C 5A AC Current Sensor Current Transformer](https://amzn.to/3DCaaU7)
    * might be able to use this for < 5 amp applications. )
3. [esp32-s2mini](https://amzn.to/3ZZC97J)
    * [documentation website](https://www.wemos.cc/en/latest/s2/s2_mini.html#documentation)
    * [Datasheet](esp32-s2_datasheet_en.pdf) 
4. [Mini Step-Down Power Supply Module](https://amzn.to/40bDZDL)
    * I think that I can use something like thios to make this so I can put it all in the same box and not have to worry about a power supply.

## Wiring
<a href="images/wiring-arduino.webp" data-lightbox="Wiring Diagram" data-title="Wiring Diagram">Wiring Diagram Graphic</a>

* Cap1 = 10uf
* R1 = 100k (anything 10k-470k)
* R2 = 100k (10k-470k)  


---

## ChatGPT Circuit Explanation  

### 1. **Current Transformer (CT Sensor: SCT-013-030)**  
- **Function**:  
  The CT sensor measures **AC current** passing through a conductor.  
- **Output**:  
  The SCT-013-030 outputs a small **AC voltage** proportional to the current being measured.  


### 2. **Burden Resistor and Capacitor**  
- The CT sensor requires a **burden resistor** across its output terminals to convert the current signal into a measurable voltage.  
- **Capacitor (10µF)**:  
  - Smooths the AC signal.  
  - Filters out **high-frequency noise** and stabilizes the voltage signal.  


### 3. **Voltage Divider (Resistor 1 and Resistor 2)**  
- The **ESP32 ADC input** cannot accept **negative voltages**.  
- To prevent this, the circuit creates a **voltage bias** at half of the ESP32's supply voltage (1.65V for 3.3V systems).  

#### How It Works:  
- Resistor 1 and Resistor 2 form a **voltage divider** that creates a steady 1.65V reference point.  
- The AC signal from the CT sensor oscillates **above and below** this 1.65V midpoint.  
- This ensures the entire waveform stays within the ESP32’s ADC range (**0V to 3.3V**).  


### 4. **ESP32 Connections**  
| **Pin**                | **Purpose**                              |  
|------------------------|-----------------------------------------|  
| **ESP32 ADC IN (Pin 34)** | Reads the conditioned voltage signal. |  
| **ESP32 GND**          | Common ground for the circuit.          |  
| **ESP32 3.3V**         | Powers the voltage divider (biasing).   |  


### 5. **How the Circuit Works**  

1. **AC Signal Processing**:  
   - The CT sensor outputs an **AC voltage signal** proportional to the measured current.  
   - The burden resistor converts current to voltage.  
   - The capacitor smooths noise.  
   - The voltage divider shifts the signal to center it around **1.65V**.  

2. **ADC Reading**:  
   - The ESP32 reads the **biased AC signal**.  
   - The signal oscillates above and below 1.65V but stays within 0V-3.3V.  

3. **Software Processing**:  
   - Remove the DC bias (1.65V) in software.  
   - Sample the waveform to calculate the **RMS value**.  
   - Use the CT sensor's calibration constant to convert the voltage reading to **current**.  


### 6. **Key Components**  

| **Component**       | **Function**                                      |  
|----------------------|--------------------------------------------------|  
| **CT Sensor**        | Detects current and outputs AC voltage.          |  
| **Burden Resistor**  | Converts current to voltage.                     |  
| **Capacitor (10µF)** | Smooths the signal and reduces high-frequency noise. |  
| **Voltage Divider**  | Creates a 1.65V bias for the AC signal.          |  
| **ESP32 ADC**        | Reads the conditioned voltage signal.            |  


### 7. **Next Steps**  

To process this signal and measure current using the ESP32:  

* [x] Use the ESP32 ADC to sample the voltage signal.  
* [x] Subtract the DC bias (1.65V) in software.  
* [x]Compute the **RMS voltage** of the AC signal.  
* [x] Use the sensor’s calibration constant to convert voltage to current.  
* [x] Buy and test  [Mini Step-Down Power Supply Module](https://amzn.to/40bDZDL)
* [ ] Buy and test [HiLetgo 2pcs ACS712 30A Current Sensor Module](https://amzn.to/3W0qi86)
* [ ] Buy and use proper fuses
* [ ] Design and 3d print box for project, (or use something off-the-shelf)
* [ ] Final Assembly
* [ ] ???
* [ ] $$$Profit$$$

---



## Web References

1. https://simplyexplained.com/blog/Home-Energy-Monitor-ESP32-CT-Sensor-Emonlib/

[A video about MQTT HomeAssistant Integration](https://youtu.be/tZjW1IHZ3Lc?si=ayxRQA0qnwDzeIZT)
{{< youtube tZjW1IHZ3Lc >}}

[A video about voltage dividers](https://youtu.be/fmSC0NoaG_I?si=9w7bYceUEaLMtL7A)

{{< youtube fmSC0NoaG_I >}}


<a href="images/powersupply.jpg" data-lightbox="ProjectPics" data-title="Power Supply Wired">Reference Pictures</a>
<a href="images/s2_mini_v1.0.0_1_16x16.jpg" data-lightbox="ProjectPics" data-title="Esp32 S2-mini Front"></a>
<a href="images/s2_mini_v1.0.0_2_16x16.jpg" data-lightbox="ProjectPics" data-title="Esp32 s2-mini Back"></a>
<a href="images/Pinout.jpg" data-lightbox="ProjectPics" data-title="Esp32 S2-mini Pinout"></a>

2. [Looks like a good reference for the safe usage of the mini step down transformer](https://randomnerdtutorials.com/esp8266-hi-link-hlk-pm03/)
    * [A safer/easier alternative](https://recom-power.com/en/products/ac-dc-power-supplies/ac-dc-off-board/rec-p-RAC05-05SK!sC14.html?0)
        * [yikes on price though](https://www.digikey.com/en/products/detail/recom-power/RAC05-24SK-C14/9695304?gclsrc=aw.ds&&utm_adgroup=Converters&utm_source=google&utm_medium=cpc&utm_campaign=Dynamic%20Search_EN_Product&utm_term=&utm_content=Converters&utm_id=go_cmp-120565755_adg-18031790235_ad-665604606899_dsa-171217885755_dev-c_ext-_prd-_sig-Cj0KCQiA7NO7BhDsARIsADg_hIY6dht0iUjawxn7lBACklt8hbWb2yPWgL0FHAHZXM6RW5sn5IP92xcaArncEALw_wcB&gad_source=1&gclid=Cj0KCQiA7NO7BhDsARIsADg_hIY6dht0iUjawxn7lBACklt8hbWb2yPWgL0FHAHZXM6RW5sn5IP92xcaArncEALw_wcB&gclsrc=aw.ds)


<a href="images/Chatgpt1.webp" data-lightbox="ChatGPTpics" data-title="ChatGPT Diagram1">Funny Diagrams that Chatgpt offered up</a>
<a href="images/chatgpt2.webp" data-lightbox="ChatGPTpics" data-title="ChatGPT Diagram2"></a>
