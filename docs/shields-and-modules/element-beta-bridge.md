# Element Beta Bridge

Helium Bridges create local connectivity for your Helium Devices and other Devices in range. A "Helium Device" is any piece of hardware - sensor, temperature gauge, wearable, etc. - that is running a Helium Module or our firmware to enable connectivity. The beta version of the Bridge, seen here, is known as the Element. 


![Element](https://www.helium.co/docs/img/element-overview.jpg)

Elements are deployed and added to the Helium Network in two simple steps: connecting it to a power source; and turning it on. (Elements connect to the internet via a cellular connection. Each comes equipped with a SIM card that is activated and managed by Heliim. Take a moment to ensure that the SIM card is properly inserted after receiving your Element.)

### Connecting to a Power Source 

The Element uses a 5V USB charger to power the board and recharge the onboard battery in case of portable operation. To plug in your Element, connect the USB Type A end of the provided power cable into the USB charger, and plug that charger into an outlet. The other end of the power cable, the USB Type B connection, plugs into bottom of the Element. 

When this is complete, you should see the LEDs on the front of the Element illuminate, indicating that the Element is now receiving power. 


### Turning on the Element 

Even though the Element is running off USB power, you should switch on the on-board battery for uninterrupted operation. To do this, open the Element case, and locate the toggle switch attached to the power wires (shown below). Toggle the switch to connect the battery. 


![Element](https://www.helium.co/docs/img/element-on-switch.jpg)


Immediately after being powered on, the blue LED located on the front of the board will go solid, indicating that the Element is in the process of connecting. This should only take a few seconds. When this is complete, the LED will go off, and then proceed to blink once per second. (In the event that the Element has weak or poor cellular signal, the device will reconnect as needed, causing the blue LED to deviate temporarily from blinking once per second.)

Your Element is now connected to the internet via a cellular connection, and ready to accept connection from Helium devices. 

You are now ready to start building applications [with your Atom](/docs/shields-and-modules/atom-beta-shield/). If you have any issues while deploying your Element, post a message on the [Forum](http://forum.helium.co/) or find us in [Helium Community Chat](http://www.hipchat.com/g0w30ttrl). 

### Element LEDs	

If your Element does not start flashing the blue light, consult the table below to decode its status:

Status | Blue Center LED | Green Center LED | Red Center LED
-------|-------------------|--------------------|-----------------
HW Reset | Solid     |                    | Flashing
Initializing    | Solid     |  Flashing          |
Connecting		| Solid    |  Solid             |

If the Element is stepping through these three phases repeatedly, you most likely do not have adequate cellular coverage in your area. Your bridge should be relocated to a spot with better reception. If this isn't an option, please email **mark@helium.co**. 

There is an additional status light located near the power connecter. When this is green, the Atom is running on AC power; when red, the battery is charging; and when it's off, the Element is running off of battery power. 