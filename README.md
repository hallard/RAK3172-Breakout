# RAK3172 breakout board

<img src="https://github.com/hallard/RAK3172-Breakout/blob/main/pictures/RAK3172-Breakout-top.png">

I'm using mainly to flash custom firmware in it, and not using AT default firmware.

**These boards have been received, assembled, and works as expected**

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

- [V1.0](https://oshpark.com/shared_projects/Ss2INNiR) 

It's a pitty after several discuss with OSHPark that I can't have any rewards for each people ordering my boards, this would allow me to order free PCB for shared projects and create new ones. For information my shared boards generated a total of **$285 162.00** orders at PCBs.io in 4 years, not bad at all :-)

Hoping one day OSHparks will thanks me giving them this market. 

### Assembled boards

**Top & bottom side V1.0**

TBD

<img src="https://github.com/hallard/RAK3172-Breakout/blob/main/pictures/RAK3172-Breakout-top.png">
<img src="https://github.com/hallard/RAK3172-Breakout/blob/main/pictures/RAK3172-Breakout-bot.png">


## Bill Of Material

Nothing fancy, all components are 0805 and/or PTH and can be ordered almost anywhere (digikey, mouser, radiospare, ...). 
use only what you need dependings on what you want to do. 

Check [BOM](https://github.com/hallard/RAK3172-Breakout/blob/main/RAK3172-Breakout-BOM.xlsx) File.

PS : 100uF 0805 capacitors C4,C5,C6 and C7 are for use with coin cell battery, no need to put them if not powering from coin. Also take care of contact is using cell coin

## License

<img alt="Creative Commons Attribution-NonCommercial 4.0" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png">   

This work is licensed under a [Creative Commons Attribution-NonCommercial 4.0 International License](http://creativecommons.org/licenses/by-nc/4.0/)    
If you want to do commercial stuff with this project, please contact [CH2i company](https://www.ch2i.eu/en#support) so we can organize an simple agreement.

