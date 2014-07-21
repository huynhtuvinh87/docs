# Helium Arduino Library


The Helium Arduino Library runs on Arduino-compatible application
nodes and provides easy access to the Helium Network.

In addition to basic data communications, we've provided functions to
support reflashing of the application CPU and functions to help
developers extract specific memory contents from a running
application.

##  Download and Installation

The easiest way to get started is to download the latest 
version of the library from 
our [Github repo](https://github.com/helium/helium-arduino/archive/master.zip). 
The Arduino GUI can load libraries directly
from the zipfile, but the Helium zipfile will not work because it
contains dashes. So unzip the downloaded file to a convenient place on
your disk, and rename the *helium-arduino-master directory* to simply
_helium_.

The library must be loaded into the Arduino development tool before
you can use it. Download the Arduino GUI from
[Arduino.cc](www.arduino.cc), and install it. Start the Arduino GUI,
and import the Helium library frome the main menu item labeled
_Sketch/Import Library/Add Library_, and select the _helium_ directory
that you renamed earlier. If there are no errors, then you should be
able to load one of the Helium example sketches. Select
_File/Examples/Helium_ to see a list of examples that you can load,
and load one of them.

Try building the sketch by pressing control-R. If all is well you will
see a message in the output panel of the Arduino IDE giving the image
size of the compiled application.  If there are problems, you can get
help resolving them on the [Arduino webpage](www.arduino.cc).

```cpp
Binary sketch size: 5,274 bytes (of a 32,256 byte maximum)
```

Now you are ready to create an Arduino application that uses the
Helium network for internet communication.  You can either modify one
of the example sketches or create a new sketch and import the Helium
library to the new sketch.

After you create a new sketch, you should importing the Arduino
library by selecting the _Sketch/Import Library/helium_
menu. Importing the library basically just adds a `#include "helium"`
directive to your sketch.


## Initialize The Helium Library

Before you can use the Helium library, you must create a single
HeliumModem object as shown in the example. The object created will be
used to exchange data with the Helium network.  This object is
referenced by the modem variable, which is a pointer the newly created
modem object.

Here is how you would create an instance of the Helium modem in the sketch’s `setup` function.


```cpp
#include <helium.h>
#include <SPI.h>

HeliumModem *modem;

void setup(void)
{
  modem = new HeliumModem();
}

```

## Get Modem Status

The `getStatus` function returns the current status of the modem. The
function will return a pointer to a ModemStatus structure, or `NULL`
if the modem failed to return the status structure.  This can happen
if the modem is powered off or if there is a hardware problem with the
connection between the modem and Arduino.


```cpp
/*
typedef struct {
    u8  type;     // MODEM_STATUS_FRAME
    u8  flags;    // Flags, b0=awake, b1=online
    u8  lqi;      // Average LQI of RF link
    u8  rate;     // Link speed, in kbits/sec
    s8  timeZone; // Local timezone, hours offset from UTC
    u32 time;     // Current time, seconds since 1/1/1970 UTC
    } ModemStatus;
*/

ModemStatus *ms = modem->getStatus();
```

`ModemStatus` structure:

|Field|Type|Meaning|
|---|---|---|
|type|uint8| Structure identifier, always set to MODEM_STATUS_FRAME |
|flags|uint8| Flags byte<br>bit0=awake<br>bit1=online|
|lqi|uint8|Average LQI of RF connection to bridge|
|rate|uint8|speed of connection, in KBits/second|
|timeZone|uint8|Local timezone, hours offset from UTC|
|time|uint32|Current time in UTC, seconds since 1/1/1970, or zero if offline|


## Send Data To The Modem

Helium uses [MessagePack](http://msgpack.org) to handle data
transmission to and from the network. The MessagePack format provides
two major advantages:

1. Data is packed efficiently (and data types are preserved)
2. Byte-swapping problems when using application hardware with
   different Endianness are handled automatically

The Helium library includes two classes to handle data: `DataPack` and
`DataUnpack`. These classes implement the MessagePack format and
simplify handling data transfers between end application (Arduino) and
the Helium system.

To send data, the user creates a `DataPack` object, and then adds
pieces of data as necessary to the object.  When the object has been
assembled by repeated calls to one of DataPack's many append commands,
the object is sent to Helium using the `sendPack` function.

The last parameter of the `DataPack` constructor is a count of
objects to be sent. These objects must be added to the object using
one of the append functions. Note that an array or map is counted as
one object, although it can contain many objects itself.


Send data to the modem with `DataPack`:

```cpp

/* Create DataPack object.  Params are:
    u8 count - Number of data objects to be sent
*/
dp DataPack(3);

// Now append the three data items to send
dp.appendU8(4);
dp.appendString("Alarm");
dp.appendArray(3);  // Third object is an array
// Add three objects to the array (can be different objects)
dp.appendU32(time);
dp.appendU8(dataIndex);
dp.appendBlock(data);


// Now send data using HeliumModem object
modem.sendPack(&dp);
```

Note that one of the objects sent can be an array or a map. Objects must be added in order. For example, to send this structure:

```cpp
struct {
    uint16 x;
    char buf[20];
    int8 yy[5];
    } foo;
```

We would make these `append` calls to add a structure named `foo`:

```cpp
dp.appendU16(foo.x);
dp.appendBlock(foo.buf,20);
dp.appendArray(5);
for (i=0;i<5;i++)
  dp.appendS8(foo.yy[i]);
```

We could skip the `appendArray` call, but that would encode
the structure below and would require space for 8 objects, not just 3:

```cpp
struct {
    int x;
    char buf[20];
    int yy1;
    int yy2;
    int yy3;
    int yy4;
    int yy5;
    };
```

The `DataPack` class offers these functions:

| Function | Params | Returns | Comments |
|---|----|---|----|
|appendArray|int count|void| Add an array of *count* objects to package.  You must append array objects separately.|
|appendMap|int count|void| Add a map of *count* pairs of objects to package.  You must append 2*count objects separately. |
|appendBlock|char *block, int len|void| Append a block of len bytes to package|
|appendU64|u64 n|void| Append a 64-bit unsigned integer to package|
|appendS64|s64 n|void| Append a 64-bit signed integer to package|
|appendU32|u32 n|void| Append a 32-bit unsigned integer to package|
|appendS32|s32 n|void| Append a 32-bit signed integer to package|
|appendU16|u16 n|void| Append a 16-bit unsigned integer to package|
|appendS16|s16 n|void| Append a 16-bit signed integer to package|
|appendU8|u8 n|void| Append a 8-bit unsigned integer to package|
|appendS8|s8 n|void| Append a 8-bit signed integer to package|
|appendBool|u8 b|void| Append a bool value (1 or 0) to package|
|appendString|char *s|void| Append a variable length asciiz string to package|
|appendFloat|float n|void| Append a single-precision floating point value to the package|

The `sendPack` function sends a block of data from the application
to the Helium Router. The data must be in the form of a DataPack
object. `sendPack` takes one parameter, a pointer to a
previously-created DataPack object.


```c
  modem->sendPack(&dp);
```

The `sendPack` function will cause the block of data to be transferred
to the Helium router.  If the block is larger than about 100 bytes,
the transfer will be broken up into multiple blocks and re-assembled
on the Helium system. (Multi-block transfers are not yet implemented.)

The `sendPack` function is a blocking function - the function will
not return until the transfer has completed, or the transfer has
failed to complete.

## Using `loop` Function To Get Modem Data


The modem must be awake before it can receive data. In a low-power
application, the modem and application node will be sleeping most of
the time, so the Helium Router will not be able to send a packet to
the modem at any given time, but must store and forward packets until
the modem is awake.  When the application wakes up and sends data, the
Router will receive the data packet, and note that this modem has
pending packets to send, and then begin sending stored packets.  In
this way there can be two-way communication between the application
and the Helium Router.

It is necessary for the application to ask for new data (called
*polling*) with the `Helium::loop` function, and to process any
received data that the function returns.  The `loop` function also
performs other tasks needed for the library, such as handling
multi-block transfers. So it should be called from the main polling
loop of the Arduino sketch.

|Function name| params | return value | Function details|
|---|---|---|---|
| loop | void | DataUnpack* | Runs recurring task functions, returns pointer to received data |

The `loop` function returns either a NULL pointer or a pointer to a
DataUnpack object.  This object contains the data received from
Helium, and the next section explains how to extract data from this
object. Important note: When `loop` returns a non-NULL pointer, it is
the caller's responsibility to delete the DataUnpack object.  Here's an example:


```cpp
// In the sketch file:
void loop (void)
{
    DataUnpack *dp = modem->loop();
    if (dp)
    {
        /******  Process input data, write your code here: ******/
        Serial.print("Got something back!\n");
        u64 v;
        if (dp->getU64(&v))
        {
            Serial.print("Received value ");
            Serial.println(v, HEX);
        }
        // User MUST delete the dp object:
        delete dp;
    }
}
```


## Unpacking Data


The `HeliumModem` object returns a pointer to a `DataUnpack` object
from its `loop` function as outlined above.  The `DataUnpack` class
stores the received data from the Helium Modem, and allows the user to
extract the data one piece at a time.  It also stores meta data about
the application data being transferred.

The `DataUnpack` class implements the following functions.  Each
function will extract one value from the msgpack object, and move the
internal pointer to the next object.  Each item must be removed in the
same order it was added on the remote side.  Each function returns an
unsigned byte (uint8) that is zero on success, or non-zero if there is
an error.

| Function | Params | Returns | Comments |
|---|----|---|----|
|getU64|u64 *val|u8| Extract one unsigned int64 value from the msgpack object|
|getS64|s64 *val|u8| Extract one int64 value from the msgpack object|
|getU32|u32 *val|u8| Extract one unsigned int32 value from the msgpack object|
| getS32|s32 *val|u8| Extract one int32 value from the msgpack object|
| getU16|u16 *val|u8| Extract one unsigned int16 value from the msgpack object|
| getS16|s16 *val|u8| Extract one int16 value from the msgpack object|
| getU8|u8 *val|u8| Extract one unsigned int8 value from the msgpack object|
| getS8|s8 *val|u8| Extract one int8 value from the msgpack object|
| getString|char *str, u16 len|u8| Extract a string from msgpack, at most `len` bytes|
| getBlock|u8 *block, u16 maxlen, u16 *len|u8| Extract a block of bytes from msgpack, at most `maxlen` bytes, actual len returned in `len`|
| getBool|u8 *val|u8| Extract a boolean from msgpack|
| getArray|u16 *count|u8| Extract an array from msgpack.  This just returns the count of the array, not the array elements.|
| getMap|u16 *count|u8| Extract a map from msgpack.  This just returns the count of the map.  This is followed by 2*count elements in the map.|


The DataPack/DataUnpack objects each use a sequence number to ensure
that duplicate packets do not get processed twice. This functionality
is handled internally, so the application can assume that a given
packet will be processed only once.


**Previously this example was given for sending some data to Helium:**

```cpp
struct {
    uint16 x;
    char buf[20];
    int8 yy[5];
    } foo;

dp.appendInt(foo.x);
dp.appendBlock(foo.buf,20);
dp.appendArray(5);
for (i=0;i<5;i++)
  dp.appendU16(foo.yy[i]);
```
**If this struct were sent from Helium, we would extract the data this way:**

```cpp
struct {
    uint16 x;
    char buf[20];
    int8 yy[5];
    } foo;

DataUnpack *dp = modem->loop();
if (dp)
{
  u16 len;
  u8 i;
  if (dp->getU16(&foo.x)) return; // Error
  if (dp->getBlock(foo.buf, 20, &len) || len != 20) return;
  if (dp->getArray(&len) || len != 5) return;
  for (i=0;i<5;i++)
    if (dp->getU8(&(foo.yy[i]))) return;

  // Caller MUST delete the DataUnpack object
  delete dp;
}
```



## Modem Sleep

The modem can be put to sleep with the `modemSleep` function.  When
the modem is sleeping, it draws only a few microamps of current. Upon
awakening, the modem is immediately able to send and receive data,
unless the wireless bridge that the modem was connected to is no
longer available.  In that case, the modem will automatically
re-connect to the Helium network, a process which usually takes about
one second.

It is necessary to wake the modem before sending data. Call
`modemSleep(1)` before creating the `DataPack` object, and the modem
will be awake when the `sendPack` function is called. If return data
is expected, the modem must be left awake until the received data has
been processed. Call `modemSleep(0)` to put the modem back to sleep.

In a low-power application it is important for the modem to be turned
off most of the time to extend battery life. One way to accomplish
this is to send data to the Helium server, and expect a receipt of any
data coming back to be done within a few hundred milliseconds at most
after sending data. The Helium server knows that a node is sleeping
based on a flag bit from previously received data, and will only send
data back to a modem immediately after receipt of data, when the modem
is still powered and receiving data.

Here is the way for a battery-powered application to send data:

1. Wake up Arduino on a timer, collect data to send, package it into a dataPack object.
2. Call `sendPack` to wake up modem and start the sending process.
3. Call `modemSleep` to put modem to sleep.
4. Put Arduino to sleep.

During the time that the application is waiting for the modem to send
the data to Helium (step 2 above), which could take several seconds or
more, the library will put the application CPU into a low-power sleep
mode during times when CPU function is not needed. If there are
peripheral devices (like A/D converters) that will draw currents
during this time, they should be turned off before calling
`sendPack`.


**Put the Helium modem to sleep:**

```cpp
modem->sleepModem(0);

```
**Wake the modem up:**

```cpp
modem->sleepModem(1);
```


## Send Debug Messages To Helium

Sometimes it may be necessary to send a debug message to the Helium network. You will be able to see these messages from the Helium Console. Here's an example:

```cpp
char msg[20];
sprintf(msg, "Hey Mom It Works!");
modem->sendDebugMsg(msg);

```


## Example Programs

We've put together a few pieces of sample code to get you started. If you've got others you want to contribute we would be happy to feature them here. Please [send us a pull request](https://github.com/nervcorp/helium-c-api-docs/pulls) and we'll take a look. 

### Simple Looping Message

```cpp
#include <SoftwareSerial.h>
#include <helium.h>

//declare the helium modem
HeliumModem *modem;

void setup() {
  modem = new HeliumModem();
}

void loop()
{
      // send a message on a loop every five seconds
      DataPack dp(1);
      dp.appendString("Sent from my Helium Device");
      modem->sendPack(&dp);
      delay(5000);
}

```

Here's your simple up-and-running example for Helium. This short program will send "Sent from my Helium Device" every five seconds. A natural extention of this in [Helium Red](https://red.helium.io) would be to pass this data to a Twitter node and send it as a tweet. You could also transform it into an SMS message and tell all your friends about Helium. 


### Button Press

This program is borrowed [from Arduino Tutorials](http://arduino.cc/en/tutorial/button). 

```cpp
#include <SoftwareSerial.h>
#include <helium.h>

//declare the helium modem
HeliumModem *modem;

const int buttonPin = 2;     // the number of the pushbutton pin

// variables will change:
int buttonState = 0;         // variable for reading the pushbutton status

void setup() {
  // initialize the pushbutton pin as an input:
  pinMode(buttonPin, INPUT);

  //initialize the modem
  modem = new HeliumModem();
}

void loop(){
  // read the state of the pushbutton value:
  buttonState = digitalRead(buttonPin);
  if (buttonState == HIGH) {
      // check if the pushbutton is pressed.
      DataPack dp(1);
      dp.appendString("Button Pressed!");
      modem->sendPack(&dp);
    }
}

```







 
