##### 1st Button: 
<i>Take a picture with 1 click:<i>
``` python
from gpiozero import Button
from picamera import PiCamera
from datetime import datetime

camera = PiCamera()
button = Button(2)

def take_picture():
    timestamp = datetime.now().strftime("%Y-%m-%d_%H-%M-%S")
    camera.capture(f"/home/aya/Desktop/image{timestamp}.jpg")

button.when_pressed = take_picture
``` 

