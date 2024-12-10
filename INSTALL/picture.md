# Raspberry Pi Button-Controlled Camera System

This guide explains how to set up a button on your Raspberry Pi to take pictures with a single click using the GPIO pins and the Raspberry Pi camera module.

---

## 1st Button: Take a Picture with 1 Click

### Python Code

```python
from gpiozero import Button
from picamera import PiCamera
from datetime import datetime

# Initialize the camera object
camera = PiCamera()

# Set up the button to use GPIO pin 2 (change the number if needed)
button = Button(2)

# Define a function to take a picture
def take_picture():
    # Get the current date and time in the format YYYY-MM-DD_HH-MM-SS
    timestamp = datetime.now().strftime("%Y-%m-%d_%H-%M-%S")
    
    # Capture a picture and save it to the Desktop with a filename that includes the timestamp
    camera.capture(f"/home/aya/Desktop/image{timestamp}.jpg")

# When the button is pressed, the take_picture function is executed
button.when_pressed = take_picture
```

---

## Explanation of Each Line

### Importing Libraries

- `from gpiozero import Button`: Imports the `Button` class from the `gpiozero` library, which is used to interface with buttons connected to the Raspberry Pi's GPIO pins.
- `from picamera import PiCamera`: Imports the `PiCamera` class from the `picamera` library, which provides an interface for controlling the Raspberry Pi camera module.
- `from datetime import datetime`: Imports the `datetime` class from the `datetime` library, which allows you to get the current date and time.

### Initializing the Camera

- `camera = PiCamera()`: Creates an instance of the `PiCamera` class, which will be used to interact with the Raspberry Pi's camera. You can now control the camera through this object.

### Setting Up the Button

- `button = Button(2)`: Creates an instance of the `Button` class and associates it with GPIO pin 2 on the Raspberry Pi. If your button is connected to a different GPIO pin, replace `2` with the appropriate GPIO pin number.

### Defining the `take_picture` Function

- `def take_picture()`: Defines a function named `take_picture`. This function will be executed when the button is pressed.

### Generating a Timestamp

- `timestamp = datetime.now().strftime("%Y-%m-%d_%H-%M-%S")`: Generates a timestamp using the current date and time. The `strftime` method formats the timestamp in the `YYYY-MM-DD_HH-MM-SS` format, ensuring unique filenames for each picture.

### Capturing and Saving the Picture

- `camera.capture(f"/home/aya/Desktop/image{timestamp}.jpg")`: Captures a picture using the camera object and saves it as a `.jpg` file. The file is saved to the desktop with a filename that includes the timestamp.

> **Note:** Replace `aya` in `/home/aya/Desktop/` with your Raspberry Pi's username. For example, if your username is `pi`, use `/home/pi/Desktop/`.

### Setting the Button’s Action

- `button.when_pressed = take_picture`: Tells the Raspberry Pi to call the `take_picture` function when the button is pressed.

---

## Additional Notes

- Ensure that the GPIO pin number in `Button(2)` matches the GPIO pin where your button is connected.
- You can run this script by saving it as a `.py` file and executing it from the terminal. It will continuously wait for the button to be pressed and take a picture each time.

### How to Run the Code

1. Save the code in a file, e.g., `camera_button.py`.
2. Open a terminal on your Raspberry Pi.
3. Navigate to the directory where you saved the file.
4. Run the script using the command:

   ```bash
   python3 camera_button.py
   ```

5. When you press the button connected to GPIO pin 2, the script will take a picture and save it to the specified directory.
