# WinchesterPCB
Open source JAMMA supergun project. KiCAD PCB design files, included are the symbols, footprints and BOM. 
The board is a two sided pcb with 1oz copper. Roughly 135mm x 86mm

# Design Breakdown

### Power
Power is supplied using a 20pin atx power connector. That way it can provide +5, -5 and +12v without having to deal with conversion on the pcb itself. In addition, it makes it easy to include a power switch and power led on the pcb. There is also logic in the atx power supply to cut power in the event of a short circuit. Which is a nice safety feature. 
  
The +5v power to the jamma connector needs to be pretty wide and beefy. If we assume 5A through a 1oz copper board thats about 5 inches, a 20 degree raise in temp puts us around 2.59mm trace width. I feel ok with the 2.5mm trace width on the board. Both the +5v and +12v are passing through an INA260 power monitoring IC. The INA260 can handle up to 36v and 15A, so its well within the use of this project. 

The teensy 4.1 uses 100mA, the oled screen 20mA, THS7376 30mA, FE1.1s 142mA, BA7230LS 50mA, and the MIC2026 is less than 1mA. So the total power consumption of the +5v coponents on the board is about 350mA. Using the .6mm trace with feels about right. 

I also made sure not to connect the +5v directly to the pins on the usb ports. The MIC2026-2YM will provide the usb ports with power according to the usb specifications. If anything goes out of spec, the MIC2026 will tell the FE1.1s usb hub to turn off the ports. This should help protect the board and the connected device, incase something goes wrong. 

### Component Video
Jamma arcade games were designed with outputting the video signal to an RGBS compatible display. And when it comes to home compatible tvs, component video is the closest to that signal. Composite video is a more common analog video input, but arcade games wont benefit from any kind of dithering effects.

The RGB signals are passed through individual potentimeters and sent to the BA7230LS to be converted to component signals. Those are then sent to a THS7376 video amplifier and connected to the sync signal which is cleaned up with an LM1881.

The THS7376 video amplifier is a drop in replacement for the THS7374. Either chip is supported in this design. Its worth noting that the 7374 uses much less power than the 7376, but Im not sure how the video quality is between the two. Im not sure if its my crt tv or my eyes, but I dont seem much different enabling and disabling the lowpass filter on the 7376.

### Teensy 4.1
The teensy 3.6 should also work for this design, but considering the 4.1 model is cheaper and faster, I dont see the point in using 3.6 unless you have one on hand. Due to the fact that the teensy 4.1 is not 5v tolerant, I have the pins connected to biased BJT transistors to connect the jamma pins to ground to signal a button press. The transistors have 1k resistors built in connecting the teensy io pin to the base and between the base and the emitter. One small design flaw is that the Player 1 Up signal is using pin 13 on the teensy, which is connected to an led. So every time player presses the up button the led lights up. I was hoping there would be a way to disable the led, but it is hard wired to the pin.

Do be careful not to have the atx power and have the teensy plugged in with the onboard micro usb port. If you do want to have both plugged in and powered at the same time use this guide to do it safely https://www.pjrc.com/teensy/external_power.html. 

### PCB
There are in pad vias in this design. If assembling by hand its not a problem, but some machines do have problems with placing the correct amount of solder the pads with vias.

Both the front and back planes of the pcb are filled with ground. I also added vias along the outside for ground stitching. I dont think its that applicable for this project, but it doesnt hurt anything either. 

All of the connectors, power switch, power led, oled display and potentiometers are using through hole designs incase someone wants to put the board inside of a case. Wires could be ran to connectors on the side of a case. 

The expansion pins are using a footprint of a flat flex connector(SLW8R-5C7LF). A pcb could be mounted on top of the right side, there is another mounting hole near the LM1881 IC. 
