### Circuit Description
## 1. First Button Connection
Connections:

Connect the positive terminal of the button to GP2 (General Purpose Pin 2, 3rd physical pin on the GPIO header).
Connect the negative terminal of the button to GND (Ground pin next to GP2, 6th physical pin).
Diagram:
Raspberry Pi Pins

    GP   GND
    O     O           
    X     O    <- Button 1 connected here
    O     X    <- GND connection
    O     O
    O     O
    
## 2. Second Button Connection
Connections:

Connect the positive terminal of the second button to GP3 (General Purpose Pin 3, 5th physical pin).
Connect the negative terminal of the button to GND (7th physical pin).
Diagram:
Raspberry Pi Pins

    GP   GND
    O     O           
    O     O
    X     O    <- Button 2 connected here
    O     O
    O     O
    O     O
    O     X    <- GND connection

## 3. Both Buttons Connected
Configuration:

Button 1 and Button 2 are connected to separate GPIO pins, sharing the same ground pins. This configuration avoids interference between the two buttons and allows the Raspberry Pi to detect presses independently.
Diagram:
Raspberry Pi Pins

    GP   GND
    O     O           
    X     O    <- Button 1
    X     X    <- Button 2 and GND shared here
    O     O
    O     O
    O     O
    O     X    <- Additional GND connection if needed

## Additional Notes
GPIO Pin Orientation:

Ensure the orientation of your Raspberry Pi matches the diagram (GPIO pins are on the top-left side when viewed from above).
Resistor Usage:

For proper debounce handling and to prevent GPIO damage, consider using pull-up or pull-down resistors (external or internal via software) with each button.
Code Integration:

Make sure your code references the correct GPIO pins:
Button 1: GP2
Button 2: GP3
This configuration is ideal for Raspberry Pi GPIO projects requiring button inputs.
