# Springer Alpha Shield


The first generation of the Helium Development Shield is known as the **Springer**, pictured below. Springers were used for the Helium Alpha program and the orginal platform testing. 

![Springer Alpha Shield](/img/springer1.jpg)


The Springers are made to pin into the [Arduino UNO](http://arduino.cc/en/Main/arduinoBoardUno), pictured below.

![Springer Alpha Shield with UNO](/img/springer-with-uno.jpg)


## Springer Arduino UNO Interfaces

The following is a description of the interfaces and pins on the Springer



|Interface | Description | 
|----| ------- | 
|B1 |	Module Reset |
|B2 | Reserved (Helium) |
|B3 |	Reserved (Helium)|
|J1	| Reserved (Future Helium Functionality)|
|JTAG | Reserved (Helium Maintenance Port)|

### Arduino Headers

The Arduino Headers (of which there are four) provide access to the programmable pins. All of these are labeled clearly on the board. 


|PIN | FUNCTION  | DESCRIPTION / NOTES|
|----|-----------|-----------------------|     
|A5  | NOT USED  | Arduino has direct control.|
|A4	 | NOT USED	 | Arduino has direct control.| 
|A3	 | NOT USED	 | Arduino has direct control.|
|A2	 | NOT USED	 | Arduino has direct control.|
|A1	 | NOT USED	 | Arduino has direct control.|
|A0	 | NOT USED	 | Arduino has direct control.|
|VIN | NOT USED	 | Arduino has direct control.|
|GND | GROUND		 | Ground Supply from Arduino.|
|GND | GROUND		 | Ground Supply from Arduino.|
|5V	 | SUPPLY FOR IO TRANSLATORS | 5V Supply from Arduino.|
|3.3V	| SUPPLY FOR RF MODULE |			3.3V Supply from Arduino.|
|RES	| RESET FOR ARDUINO (5V LEVEL) |		Module can reset the Arduino with this pin.  Helium library pre-configures this pin.  Not currently implemented.  Future Over The Air Reset feature. |
|RX		|	MODULE USART0 RECEIVE PIN			|	Not implemented.  Shared with Arduino RX pin for serial boot loading (image reflashing).  This pin can be used for customer applications if needed (bypass module). |
|TX		|	MODULE USART0 TRANSMIT PIN		|		Not implemented.  Shared with Arduino TX pin for serial boot loading (image reflashing).  This pin can be used for customer applications if needed (bypass module). |
|D2		|	SLEEP/GPIO2 (5V LEVEL)			|		Sleep pin for the Module (GPIO pin from Module pin 13). Low (0V) = Sleep.  High (5V) = Wake.  Helium library samples. |
|D3		|	GPIO3 (5V LEVEL)			|				Reserved for Future use.  GPIO pin from Module pin 12.  Helium library controls. |
|D4		|	NOT USED						|		Arduino has direct control. | 
|D5		|	BUZZER1						|		Direct Buzzer control pin 1 for Arduino. |
|D6		|	BUZZER2						|		Direct Buzzer control pin 2 for Arduino. |
|D7		|	RESET FOR MODULE (5V LEVEL)		|		Arduino can reset the module with this pin.  Combined with B1. |
|D8		|	SPI SPIO (5V LEVEL)			|	Module uses this pin to update Arduino with current SPI status.  Controlled by Helium Library.|
|D9		|	*(RF TEST ONLY PIN)			|	Reserved. Not used for normal operation. |
|D10	|		SPI SS (5V LEVEL)			|	This is the SPI Slave Select Pin.  Helium library controls. |
|D11	|		SPI MOSI (5V LEVEL)		|	This is the SPI MOSI Pin. Controlled by Helium Library. |
|D12	|		SPI MISO (5V LEVEL)		|	This is the SPI MISO Pin. Controlled by Helium Library. |
|D13	|		SPI CLOCK (5V LEVEL)	|	This is the SPI CLOCK Pin.  Controlled by Helium Library..|
|GND	|		GROUND								|	Ground Supply from Arduino. |
|AREF	|	NOT USED						|		Arduino has direct control.|
