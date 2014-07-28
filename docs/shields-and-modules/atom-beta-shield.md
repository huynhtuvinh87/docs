# Atom Beta Shield

The second generation of the Helium Development Shield is known as the **Atom**. The Atom, pictured here, is what beta participants are using for their development on the Helium network. 


![Atom Beta Shield](https://www.helium.co/docs/img/atom1.jpg)


The Atoms are made to pin into the [Arduino UNO](http://arduino.cc/en/Main/arduinoBoardUno), pictured below. (Atoms will work with various other MCUs that are Arduino-compatible, but the UNO provides seamless integration with the Atom's pinout. Other boards like the Intel Galileo and Arduino Mega will require jumper wires for integration.) 


![Atom Beta Shield](https://www.helium.co/docs/img/atom-with-uno.jpg)

## Connecting Your Atom

If you've just taken delivery on your Atom, the easiest way to get it connected to the network is with Fusion. [Start here](/fusion/helium-fusion/).

## Atom Schematic 

You can find a full schematic for the Atom [here](https://www.helium.co/docs/assets/Springer_Rev2_0_SCH.pdf) (PDF). This should aid you when integrating with the various available pins on the Atom.  

## Atom LEDs 

The Atom has three LEDs:

|LED COLOR  | DESCRIPTION									   | 
|-----------|--------------------------------------------------|
|Green      | Power indicator. This will turn off during sleep.| 
|Red        | RX indicator									   |	 
|Blue       | TX indicator									   | 

During power on, the Blue LED will flash very rapidly, showing it is scanning channels. If a Bridge is within range, it will enter the association process. Upon completion of that process, the Bridge will send the final configuration data and the Atom's Red LED will stay lit for 500ms. After this is complete, the red LED will continue blinking once per second to signify that it's online and listening to the Bridge. 

If there is an application that requires transmitting data from the Atom to the network, the Blue LED will light up, signifying the transmitted frames leaving the Atom. This will be a 500ms blink when data is transmitted. If sleep is used - done so by toggling the `D5` pin - the Atom will enter sleep mode and the module will not receive or transmit any data. Once awake, the Atom will again be listening for the Bridge data and start sending application data as required. If the Bridge is moved or out of range, the Atom will enter re-scaning mode to search for the nearest Bridge again.

## Atom Pins

The Atom Shield Pin Mapping is as follows: 

|FUNCTION   | PIN  | DESCRIPTION / NOTES           					  |
|-----------|------|--------------------------------------------------|
|USART RXD  | D7   | Input TX'd data from Arduino to Module           | 
|USART TXD  | D6   | Output TX'd data from Module to Arduino          | 
|SLEEP      | D5   | Input from Arduino to Sleep Module (ACTIVE HIGH) |
|RESET      | D4   | Input from Arduino to Reset Module (ACTIVE HIGH) |
|IO BUTTON  | D3   | Output from button B3 to Arduino (selected by J9)|


**NOTE: All selected IO configurations can be changed by moving the jumpers  `J2`, `J3`, `J4`, `J5`, and `J9` and manually jumping wires to the desired application pins on the Arduino.**

