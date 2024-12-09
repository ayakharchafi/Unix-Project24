# Install Nginx as a Reverse Proxy and Install PiGallery2 and Run in Docker

This guide outlines how to set up Nginx as a reverse proxy, install PiGallery2, and run it in Docker on a Raspberry Pi. It includes steps for Docker installation, PiGallery2 setup, Nginx configuration, and system service setup for automatic startup.

## 1. Install Docker and Docker Compose
Docker is required to run PiGallery2 in a containerized environment. Docker Compose will help define and manage multi-container applications.
   - **a.** Install Docker by running the command to download and install the Docker script.
     ```bash
     curl -sSl https://get.docker.com | sh
     ```
     This fetches the latest Docker installation script and runs it on your Raspberry Pi.
   - **b.** Add your user to the `docker` group to run Docker commands without `sudo`.
     ```bash
     sudo usermod -aG docker $(whoami)
     ```
   - **c.** Reboot the Raspberry Pi to apply the group change.
     ```bash
     sudo reboot
     ```
   - **d.** Update the system’s package list.
     ```bash
     sudo apt-get update
     ```
   - **e.** Install `python3-pip`, which is required to install Docker Compose.
     ```bash
     sudo apt-get install -y python3-pip
     ```
   - **f.** Install Docker Compose using `pip3`.
     ```bash
     sudo pip3 install docker-compose
     ```

## 2. Set Up PiGallery2 in Docker
PiGallery2 is an image gallery web app, and running it in a Docker container makes deployment easier.
   - **a.** Create a directory for PiGallery2 configuration.
     ```bash
     mkdir ~/pigallery2
     ```
   - **b.** Navigate to the PiGallery2 directory.
     ```bash
     cd ~/pigallery2
     ```
   - **c.** Create a `docker-compose.yml` file to define the PiGallery2 container configuration.
     ```bash
     nano docker-compose.yml
     ```
     Add the following content to the file:
     ```yaml
     version: '3'

     services:
       pigallery2:
         image: bpatrik/pigallery2:latest
         container_name: pigallery2
         environment:
           - PG2_DATABASE_URL=sqlite:///home/pigallery2/pigallery2.db
           - PG2_DATABASE_TYPE=sqlite
           - PG2_UPLOADS_DIR=/uploads
           - PG2_STATIC_DIR=/static
           - PG2_PORT=80
         volumes:
           - ./uploads:/uploads
           - ./static:/static
           - ./config:/config 
         ports:
           - "5000:80"
         restart: always
     ```
     This YAML file defines a `pigallery2` service that uses the latest PiGallery2 Docker image. It sets up necessary environment variables, volumes, and port forwarding.
   - **d.** Start PiGallery2 container using Docker Compose.
     ```bash
     docker-compose up -d OR docker compose up -d
     ```
     This command pulls the PiGallery2 image, starts the container, and runs it in detached mode.
   - **e.** Verify that the Docker container is running.
     ```bash
     docker ps
     ```
     Alternatively, open the PiGallery2 web interface in the browser:
     ```
     http://rpi-ip:5000
     ```

## 3. Install and Configure Nginx as a Reverse Proxy
Nginx will forward requests from port 80 to the PiGallery2 container running on port 5000.
   - **a.** Update system packages.
     ```bash
     sudo apt-get update
     ```
   - **b.** Install Nginx.
     ```bash
     sudo apt-get install nginx
     ```
   - **c.** Create an Nginx configuration file for PiGallery2.
     ```bash
     sudo nano /etc/nginx/sites-available/pigallery2
     ```
     Add the following Nginx configuration:
     ```nginx
     server {
         listen 80;

         server_name rpi-ip;

         location / {
             proxy_pass http://rpi-ip:5000;
             proxy_set_header Host $host;
             proxy_set_header X-Real-IP $remote_addr;
             proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
             proxy_set_header X-Forwarded-Proto $scheme;
             proxy_http_version 1.1;
             proxy_cache_bypass $http_upgrade;
         }
     }
     ```
     This configuration sets up Nginx to forward requests from `http://rpi-ip` to the PiGallery2 Docker container on port 5000.

## 4. Enable the Configuration
   - **a.** Create a symbolic link to enable the Nginx configuration.
     ```bash
     sudo ln -s /etc/nginx/sites-available/pigallery2 /etc/nginx/sites-enabled/
     ```

## 5. Test Configuration
   - **a.** Test the Nginx configuration for syntax errors.
     ```bash
     sudo nginx -t
     ```

## 6. Restart Nginx
   - **a.** Restart Nginx to apply the configuration changes.
     ```bash
     sudo systemctl restart nginx
     ```
   - **b.** Verify that PiGallery2 is accessible via the web interface.
     ```
     http://rpi-ip
     ```

## 7. Systemd Service for Docker Compose
Create a `systemd` service to ensure that PiGallery2 starts automatically with Docker Compose on boot.
   - **a.** Create the systemd service file.
     ```bash
     sudo nano /etc/systemd/system/pigallery2.service
     ```
     Add the following content:
     ```ini
     [Unit]
     Description=PiGallery2 Docker Compose Service
     Requires=docker.service
     After=docker.service

     [Service]
     WorkingDirectory=/home/aya/pigallery2
     ExecStart=/usr/bin/docker/docker-compose up -d
     Restart=always
     User=aya
     Group=aya
     Type=oneshot
     RemainAfterExit=true

     [Install]
     WantedBy=multi-user.target
     ```
     This file configures the PiGallery2 service to start with Docker Compose when the system boots up.

## 8. Reload systemd to Recognize the New Service
   - **a.** Reload the systemd manager configuration.
     ```bash
     sudo systemctl daemon-reload
     ```

## 9. Enable Service to Start When the Raspberry Pi Boots
   - **a.** Enable the PiGallery2 service to start on boot.
     ```bash
     sudo systemctl enable pigallery2
     ```

## 10. Start Service
   - **a.** Start the PiGallery2 service.
     ```bash
     sudo systemctl start pigallery2
     ```

## 11. Reboot Raspberry Pi
   - **a.** Reboot the Raspberry Pi to ensure all services are properly started.
     ```bash
     sudo reboot
     ```

## 12. Verify if Everything is Working
   - **a.** Check the status of the Docker container.
     ```bash
     docker ps
     ```
   - **b.** Check the status of the PiGallery2 service.
     ```bash
     systemctl status pigallery2
     ```
   - **c.** Open the PiGallery2 web interface in the browser.
     ```
     http://rpi-ip
     ```

---

This setup allows you to access PiGallery2 through the Nginx reverse proxy and ensures that it automatically starts on boot.
