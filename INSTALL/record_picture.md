# Capture Photos and Record Videos with GPIO Buttons
### 1st and 2nd Button
<i>Take a picture with 1 click + Record with another button with 2 clicks</i>

This script allows you to control a Raspberry Pi camera module using two buttons. One button is used to take pictures, while the other is used to start and stop video recording with a double click. Below is a detailed explanation for each part of the script.

```python
from gpiozero import Button  # Library to interface with GPIO pins and handle button inputs
from signal import pause  # Keeps the program running to wait for events
import time  # Used to track time between button clicks
from picamera import PiCamera  # Library for controlling the Raspberry Pi camera module
from datetime import datetime  # Provides current date and time for filenames

# Set up buttons connected to GPIO pins
button = Button(2)  # Button 1 connected to GPIO pin 2, used for recording control

deux = Button(3)  # Button 2 connected to GPIO pin 3, used for taking pictures

# Initialize the camera object
camera = PiCamera()  # Create an instance of the PiCamera class to interact with the camera module

# Function to get the current timestamp
def get_timestamp():
    """Generates a timestamp in the format YYYY-MM-DD_HH-MM-SS."""
    return datetime.now().strftime("%Y-%m-%d_%H-%M-%S")

# Function to take a picture
def take_picture():
    """Captures a picture and saves it to a file with a timestamped filename."""
    # Generate a unique filename using the current timestamp
    timestamp = get_timestamp()
    file_path = f"/home/aya/Desktop/PiPic/img_{timestamp}.jpg"  # File path for the image

    # Capture the picture and save it to the specified location
    camera.capture(file_path)
    print(f"Picture taken and saved to {file_path}")  # Provide feedback in the console

# Assign the take_picture function to the second button
deux.when_pressed = take_picture  # Executes take_picture() when the second button is pressed

# Variables to manage recording state and button clicks
recording = False  # Tracks whether the camera is currently recording
clicks = 0  # Tracks the number of button presses
last_click_time = 0  # Stores the timestamp of the last button press

# Function to toggle video recording
def toggle_recording():
    """Starts or stops video recording based on the current recording state."""
    global recording  # Access the global recording variable
    recording = not recording  # Toggle the recording state

    if recording:
        # Start recording a video with a unique filename
        timestamp = get_timestamp()
        video_file = f"/home/aya/Desktop/video_{timestamp}.h264"
        camera.start_recording(video_file)
        print(f"Recording started: {video_file}")  # Provide feedback in the console
    else:
        # Stop recording the video
        camera.stop_recording()
        print("Recording stopped.")

# Function to handle button clicks for recording
def button_clicked():
    """Handles single and double clicks to toggle video recording."""
    global clicks, last_click_time  # Access global variables
    current_time = time.time()  # Get the current time in seconds

    if current_time - last_click_time <= 0.5:  # Check if the second click is within 0.5 seconds
        clicks += 1  # Increment the click count
    else:
        clicks = 1  # Reset click count if the interval is too long

    last_click_time = current_time  # Update the timestamp of the last click

    if clicks == 2:  # If a double-click is detected
        toggle_recording()  # Toggle recording state
        clicks = 0  # Reset the click count

# Assign the button_clicked function to the first button
button.when_pressed = button_clicked  # Executes button_clicked() when the first button is pressed

# Keep the program running to detect button presses
pause()  # Prevents the script from terminating immediately
```

### Explanation of the Code:

#### Libraries Used:
- `gpiozero`: Simplifies interaction with GPIO pins for handling button presses.
- `signal`: Provides the `pause` function to keep the script running indefinitely.
- `time`: Helps in measuring time intervals between button presses.
- `picamera`: Controls the Raspberry Pi camera module to capture images and record videos.
- `datetime`: Generates timestamps for unique filenames.

#### Setting Up Buttons:
- `Button(2)`: Represents the button connected to GPIO pin 2, used for toggling video recording.
- `Button(3)`: Represents the button connected to GPIO pin 3, used for taking pictures.

#### Camera Initialization:
- `PiCamera()`: Creates a camera object to control the Raspberry Pi camera module.

#### Functions:
1. **`get_timestamp()`**:
   - Generates a formatted timestamp (e.g., `2024-12-09_14-30-00`) to ensure unique filenames.

2. **`take_picture()`**:
   - Captures an image and saves it to a file in the `PiPic` folder on the Desktop with a timestamped filename.
   - Provides console feedback to confirm the action.

3. **`toggle_recording()`**:
   - Toggles the recording state (start/stop) and generates a timestamped filename for the video.
   - Provides console feedback to indicate whether recording has started or stopped.

4. **`button_clicked()`**:
   - Handles single and double-click logic to toggle video recording.
   - Tracks the time between clicks and determines if a double-click occurred.

#### Key Logic:
- `button.when_pressed`: Assigns specific functions to execute when a button is pressed.
- `pause()`: Keeps the script running to continuously listen for button presses.

### How to Use:
1. Connect two buttons to GPIO pins 2 and 3 on the Raspberry Pi.
2. Copy the script into a file named `camera_control.py`.
3. Run the script in the terminal using:
   ```bash
   python3 camera_control.py
   ```
4. Press Button 1 (GPIO pin 2) to start/stop recording with a double click.
5. Press Button 2 (GPIO pin 3) to take a picture.

### Notes:
- Ensure that `/home/aya/Desktop/PiPic/` exists on your Raspberry Pi or update the paths in the script.
- Replace `aya` in the file paths with your Raspberry Pi username, if different.
- Test button connections and GPIO pin numbers to match your setup.

This script provides a simple yet powerful way to control your Raspberry Pi camera module for image capture and video recording.
