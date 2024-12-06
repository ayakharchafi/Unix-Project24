##### 1st and 2nd Button
<i>Take a picture with 1 click + Record with another button with 2 clicks<i>
```python
from gpiozero import Button
from signal import pause
import time
from picamera import PiCamera
from datetime import datetime

button = Button(2)
deux = Button(3)
camera = PiCamera()

def get_timestamp():
    return datetime.now().strftime("%Y-%m-%d_%H-%M-%S")

def take_picture():
    timestamp = get_timestamp()
    file_path = f"/home/aya/Desktop/img_{timestamp}.jpg"
    camera.capture(file_path)
    
deux.when_pressed = take_picture

recording = False
clicks = 0
last_click_time = 0

def toggle_recording():
    global recording
    recording = not recording
    if recording:
        timestamp = get_timestamp()
        video_file = f"/home/aya/Desktop/video_{timestamp}.h264"
       
        camera.start_recording(video_file) 
    else:
        camera.stop_recording() 

def button_clicked():
    global clicks, last_click_time
    current_time = time.time()
    
    if current_time - last_click_time <= 0.5:
        clicks += 1
    else:
        clicks = 1
    
    last_click_time = current_time
    
    if clicks == 2:
        toggle_recording()
        clicks = 0
        
button.when_pressed = button_clicked
pause()
```
