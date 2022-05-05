# efm8-arduino-uno-programmer
Program EFM8 devices using an Arduino UNO.

Thanks to jaromir-sukuba, and racerxdl for working on firmware to implement C2 protocol via arduino GPIO.  This work largely pulls from them, as well as 
connorpp.

## Pre-Steps
C2 is a 2-pin protocol.  Any arduino should work to implement the protocol via GPIO.  Just make sure that the correct pins are mapped for your particular Arduino board. Currently, Arduino UNO maps C2D and C2CK to digital pins 5 and 6, respectively. 

### Update Pins
Check the [firmware file](https://github.com/xkey10x/efm8-arduino-programmer/blob/master/prog/emf8prog.ino#L11) and change the pins to map to your device if needed.

### Arduino Mega support
To use it for Arduino Uno you can just swap from D to E. Just replace all `PORTD`, `DDRD` and `PIND` with `PORTE`, `DDRE` and `PINE`. You can read about Port Registers here: https://www.arduino.cc/en/Reference/PortManipulation

### Write firmware to Arduino
Program the firmware to the arduino and connect C2D, C2CK, and GND to your target device.

## Setup for flashing EFM8
It is set up in a client/server model to be able to easily support programming multiple targets at the same time.

### Requirements
- You need to have Python (2.7) installed.
- Then, install some required python modules.

```
pip install -r requirements.txt
```

### Setup of Server & Client

#### Server 
First you must run the server that will handle communication to the arduino.

```
python prog_server.py <serial-port> [<serial-port2> ...]
```

Supply a list of serial ports (you can use more than one Arduino at a time) to handle programming.  E.g /dev/USB1 or COM1 or /dev/cu.usbmodem*.

#### Client
Then you can finally program something using the client script.

```
python prog_client.py <serial-port> <firmware.hex>
```

This will connect to the server and tell it to download specified firmware via specified arduino.

## Troubleshooting

- If your server can't start make sure you have port 4040 available
- If you get python errors make sure you're not running python3
- Some modules need sudo on some systems
- If you're getting data errors, reduce baud rate: https://github.com/conorpp/efm8-arduino-programmer/issues/5
