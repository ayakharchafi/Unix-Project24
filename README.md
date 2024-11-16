# Unix-Project24
# Raspberry Pi Portable Camera System

## Project Overview

This project aims to create a portable camera system using the **Raspberry Pi 4 Model B** and **Camera Module 2**. The system will allow users to capture images and videos with the push of a button and store these media files on a web server. The camera will be housed in a custom 3D-printed case and mounted on a harness for easy portability, making it ideal for activities such as hiking, biking, or even as a wearable GoPro alternative.

### Key Features:
- **Picture and Video Capture:** Buttons for taking photos and recording videos.
- **Remote Access:** View captured media via a web server hosted on the Raspberry Pi.
- **Media Customization:** Apply filters or crop images/videos using command-line tools.
- **Portable Design:** Mounted on a body harness for easy wear and use.

## Platform

The project is built on the **Raspberry Pi 4 Model B**, running **Raspberry Pi OS** (Linux). The platform allows for easy customization with GPIO controls for button input, Camera Module integration, and the setup of a lightweight web server.

## Requirements

### Hardware Components:
- **Raspberry Pi 4 Model B**
- **Camera Module 2**
- **Micro SD Card** for OS and media storage
- **HDMI Cable** for setup
- **Power Supply Cable** for reliable power
- **Tactile Buttons (3)**: For picture capture, video recording, and power control
- **Jumper Wires & Breadboard** for GPIO connections
- **Portable Power Source** for camera mobility
- **3D Printed Custom Case** to house components
- **Connectivity Module** for internet access
- **Cooling (Heat sinks/Fan)** for temperature regulation
- **Straps/Harness** for stable mounting on a user's body

### Software Requirements:
- **Camera Drivers** for Camera Module 2
- **Python Libraries** for GPIO and camera control (`picamera`)
- **Web Server Software** (Nginx)
- **Gallery Software** (Lychee, PiGallery2, or PhotoPrism)
- **Image Processing Software** (ImageMagick, OpenCV)

## Setup Guide

### 1. Raspberry Pi Setup
- **Install Raspberry Pi OS** on the micro SD card.
- **Connect the Camera Module 2** and test it using simple commands to capture still images or videos.

### 2. Button Integration
- **Wire Tactile Buttons** to the Raspberry Pi’s GPIO pins.
- **Write Python Scripts** for button functionality:
  - **Picture Button**: Capture images.
  - **Video Button**: Record/stop video.
  - **Power Button**: Power on/off the system.

### 3. Web Server Setup
- **Install a Web Server** (e.g., Nginx) to host media files.
- **Gallery Software**: Install a photo gallery software (Lychee, PiGallery2, or PhotoPrism) for media management.
- **Upload Media**: Configure the system to upload images to an external server for remote access.

### 4. Portable Camera Build
- **Design & Print the Case**: Use a 3D printer to create a custom case for the Raspberry Pi and Camera Module.
- **Attach the Camera to a Harness**: Ensure the camera is stable and comfortable to wear.

### 5. Additional Enhancements
- **Cooling System**: Install a heat sink for temperature regulation.
- **Time of Flight (ToF) Sensor**: Optionally, add a ToF sensor to measure the distance between the camera and objects.

## Security & Authentication

- **RSA Key-based Authentication**: Secure access to the Raspberry Pi via SSH.
- **File Permissions**: Use `chmod` to control access to photos and videos.
- **Firewall Configuration**: Use `ufw` to configure basic firewall rules.
- **Systemd Services**: Automate system startup and camera readiness.

## Media Customization

- **ImageMagick**: For applying filters and editing images from the command line.
- **OpenCV**: For advanced processing, such as face recognition and object detection (optional).

## Power Solutions

- **Portable Battery Pack**: To ensure the camera operates for long durations without requiring external power.
- **Solar Charger** (Optional): Use a solar charger to keep the battery topped up during outdoor activities.

## Major Technical Decisions

- **Storage Options**: We chose an **SD card** for local storage as it provides sufficient space and is cost-effective. If necessary, cloud storage can be considered in the future.
- **Cooling**: We decided to use a **heat sink** for cooling, which is efficient and doesn’t add moving parts that could malfunction.
- **Web Server**: We selected **Nginx** as it is lightweight and works well with the Raspberry Pi’s hardware constraints.

## Future Enhancements

- **Cloud Storage**: If the system gains popularity, we might switch to cloud storage for media backups.
- **Time of Flight (ToF) Sensor**: To measure distances for better picture quality.

## Timeline

- **Week 1**: Setup the Raspberry Pi, Camera Module, and buttons. Start the web server.
- **Week 2**: Test button functionality, finalize the server, and begin 3D printing the case.
- **Week 3**: Finalize the case, assemble the camera, and complete the project presentation.

## Demo Plan

- **Demonstration**: We will show the fully assembled portable camera system, with working buttons for photo and video capture. The captured media will be accessible via the web interface hosted on the Raspberry Pi.

## Conclusion

This Raspberry Pi-based portable camera will provide a versatile, easy-to-use camera system for anyone looking to record their activities without holding a camera. With the added features of remote access, media customization, and portability, this camera can serve as a valuable tool for adventurers or anyone looking for a hands-free recording solution.

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
