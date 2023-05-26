# RAK3172 breakout board

<img src="https://github.com/hallard/RAK3172-Breakout/blob/main/pictures/RAK3172-Breakout-top.png">

Based on [RAK3172](https://docs.rakwireless.com/Product-Categories/WisDuo/RAK3172-Module/Overview/) from RAK Wireless.

I'm using mainly to flash custom firmware in it, and not using AT default firmware.

##Changes log

**V1.1**

- Fixed Boot0 wiring to 3V3 instead of ground
- Fixed BOM on schematics 

**V1.0**

- RAK3172 Module
- No USB/Serial, SMD FTDI 6 pins connector (**use 3.3V FTDI One, not 5V**)
- Exposed JTAG pins needed to flash module (PA13-SWDIO / PA14-SWCLK / RESET)
- Red Led on PB5
- Green Led on PB10
- Optional 2 Tactile Switch (boot and reset)
- I2C 4 pins location for "classic" sensors

## Detailed Description

No specific documentation for now, it's just a kind of wiring helper as schematic


## Schematic

<img src="https://github.com/hallard/RAK3172-Breakout/blob/main/pictures/RAK3172-Breakout-sch.png">

## Boards 

You can order the board on [oshpark](https://oshpark.com). 

- [V1.1](https://oshpark.com/shared_projects/s3n0gTxi) 
- [V1.0](https://oshpark.com/shared_projects/Ss2INNiR) 


It's a pitty after several discuss with OSHPark that I can't have any rewards for each people ordering my boards, this would allow me to order free PCB for shared projects and create new ones. For information my shared boards generated a total of **$285 162.00** orders at PCBs.io in 4 years, not bad at all :-)

Hoping one day OSHparks will thanks me giving them this market. 

### Assembled boards

**Top & bottom side**

TBD

<img src="https://github.com/hallard/RAK3172-Breakout/blob/main/pictures/RAK3172-Breakout-top.png">
<img src="https://github.com/hallard/RAK3172-Breakout/blob/main/pictures/RAK3172-Breakout-bot.png">


## Bill Of Material

Nothing fancy, all components are 0805 and/or PTH and can be ordered almost anywhere (digikey, mouser, radiospare, ...). 
use only what you need dependings on what you want to do. 

Check [BOM](https://github.com/hallard/RAK3172-Breakout/blob/main/RAK3172-Breakout-BOM.xlsx) File.

PS : 100uF 0805 capacitors C4,C5,C6 and C7 are for use with coin cell battery, no need to put them if not powering from coin. Also take care of contact is using cell coin


### Test Board out of factory stock 

When the boards are from factory, default AT firmware is flashed and thus we have the possibility to test the board before flashing custom firmware and maily also get defaults keys from device.

To do so, connect a 3V3 FTDI Type USB/Serial to access Serial Console 

> :warning: **Do not use 5V configured FTDI** 

I personnaly use these one for [Sparkun](https://www.sparkfun.com/products/14050) but you can find clones anywhere on the Web.

![](https://cdn.sparkfun.com/assets/parts/1/1/8/8/8/14050-01.jpg)

Once done open Serial terminal (the one from FTDI Serial Port) configured as `9600` BPS `8N1`, no flow control, echo typed characters and set to CR+LF for enter key, press reset button and you should be able to see banner

```
LoRa (R) is a registered trademark or service mark of Semtech Corporation or its affiliates. LoRaWAN (R) is a licensed mark.

______  ___   _   __  _    _ _          _               
| ___ \/ _ \ | | / / | |  | (_)        | |              
| |_/ / /_\ \| |/ /  | |  | |_ _ __ ___| | ___  ___ ___ 
|    /|  _  ||    \  | |/\| | | '__/ _ \ |/ _ \/ __/ __|
| |\ \| | | || |\  \ \  /\  / | | |  __/ |  __/\__ \__ \
\_| \_\_| |_/\_| \_/  \/  \/|_|_|  \___|_|\___||___/___/
========================================================
RAK3172-H Version:v1.0.2 May 26 2021
Current Work Mode: LoRaWAN.
```

Then type `AT` command to see if the RAK board answer, in this example the board answered `OK` which is correct
```
AT 
OK
```

Now get the device version
```
AT+VER=? 
V1.0.2
OK
```

Now get the device keys information 
```
AT+DEVEUI=? 
ac1f09fffe0527f5
OK
AT+APPEUI=? 
ac1f09fff8683172
OK
AT+APPKEY=? 
ac1f09fffe0527f5ac1f09fff8683172
OK
```

I'm using [TTN](https://www.thethingsnetwork.org/) for testing so please follow excellent RAK guide on how to provision your device onto TTN [here](https://docs.rakwireless.com/Product-Categories/WisDuo/RAK3172-Module/Quickstart/#connecting-to-the-things-network-ttn)

In our case we will use the AppKey generated from TTN when provisionning device, just provision your device onto TTN, get the key and put into the device as follow with command `AT+APPKEY` in our case AppKey is `B3D2F9587DED7B03AD9F1809564192E0`
```
AT+APPKEY=B3D2F9587DED7B03AD9F1809564192E0 
OK
```
Check it's ok
```
AT+APPKEY=?                                
b3d2f9587ded7b03ad9f1809564192e0
OK
```

Set LoRaWAN Mode + OTAA + Class A + Frequency Plan EU868 (Band 4) + ADR
```
AT+NWM=1 
OK

AT+NJM=1 
OK

AT+CLASS=A 
OK

AT+BAND=4 
OK

AT+ADR=1 
OK
```

Now time to join (be sure device is provisioned on TTN and you have a TTN gateway around)
```
AT+JOIN=1:0:10:8 
OK
```

some seconds later you should have confirmation
```
+EVT:JOINED
```

Now send ASCII "1234" confirmed message
```
AT+SEND=2:31323334 
OK
+EVT:SEND CONFIRMED OK
```

And here we go, all is working fine, now time to have some fun with custom firmware


### Compile and flash Firmware

You can flash the board with excellent [mbed-os](https://os.mbed.com/mbed-os/) framework. 
Easy way is to use [mbed studio IDE](https://os.mbed.com/studio/). 
We added this board into [stm32customtargets](https://github.com/ARMmbed/stm32customtargets), don't hesitate to read the [readme](https://github.com/ARMmbed/stm32customtargets/blob/master/README.md). 
Finally the main firmware [mbed-os-example-lorawan](https://github.com/ARMmbed/mbed-os-example-lorawan) program.

Once IDE installed: 

- use `file` / `import program` and them import the example with URL `https://github.com/ARMmbed/mbed-os-example-lorawan`
- right click in the project name and select `Add Library` and enter `https://github.com/ARMmbed/stm32customtargets`
- open the file `custom_targets.json` from folder `stm32customtargets` and copy whole contents
- paste copied contents in the main root folder file `custom_targets.json` (yes replace the whole file) 
- open the file `mbed_app.json` and change parameters on the section `target_overrides`
    - LoRaWAN parameters such as frequency plan, OTAA, Duty Cycle, ...
    - replace keys with the ones you got from above step `lora.device-eui`, `lora.application-eui` and `lora.application-key`
- add the following section near the end of the file `mbed_app.json`.

```json
        "RAK3172_BREAKOUT": {
            "stm32wl-lora-driver.rf_switch_config": 2,
            "stm32wl-lora-driver.crystal_select": 0,
            "stm32wl-lora-driver.debug_rx": "LED1",
            "stm32wl-lora-driver.debug_tx": "LED2",
            "stm32wl-lora-driver.debug_invert": 1
        }
```

Then on IDE select target "RAK3172_BREAKOUT", build and flash with your favorite programmer (I'm using STLink) with GND/SWDIO/SWDCLK/RESET connected. 

### Build and Flash

From IDE you can build the example. If you plug your STLink while project opened, mbed ide will ask you if you want to set it up for this project/target, once approved you can compile, flash and even debug from mbed ide (need some tools installed, [read](https://os.mbed.com/docs/mbed-studio/current/monitor-debug/debugging-with-mbed-studio.html), very nice.


<img src="https://github.com/hallard/RAK3172-Breakout/blob/main/pictures/RAK3172-Breakout-mbed-ide.png">

You can also see logs with the FTDI adapter and any Serial terminal set to 115200 bauds 8 bits no parity 1 stop bit (8N1)

```
Mbed LoRaWANStack initialized 
 CONFIRMED message retries : 3 
 Adaptive data  rate (ADR) - Enabled 
 Connection - In Progress ...
 Connection - Successful 
 Dummy Sensor Value = 3 
 23 bytes scheduled for transmission 
 Message Sent to Network Server 
 Dummy Sensor Value = 5 
 23 bytes scheduled for transmission 
 Message Sent to Network Server 
 Dummy Sensor Value = 7 
 23 bytes scheduled for transmission 
```

Green LED will be on when on receive mode and Red when sending data.

## License

<img alt="Creative Commons Attribution-NonCommercial 4.0" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png">   

This work is licensed under a [Creative Commons Attribution-NonCommercial 4.0 International License](http://creativecommons.org/licenses/by-nc/4.0/)    
If you want to do commercial stuff with this project, please contact [CH2i company](https://www.ch2i.eu/en#support) so we can organize an simple agreement.

