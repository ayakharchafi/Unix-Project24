#### Install Nginx as a Reverse Proxy and Install PiGallery and Run in a docker
``` cmd
1- Install Docker and Docker compose (Define your applications containers as code inside a YAML file you can commit to your source repository. You can start all your containers with a single command)
    a. curl -sSl https://get.docker.com | sh
    b. sudo usermod -aG docker $(whoami)
    c. sudo reboot
    d. sudo apt-get updaye
    e. sudo apt-get install -y python3-pip
    f. sudo pip3 install docker-compose

2-Set Up PiGallery2 in Docker
    a. mkdir ~/pigallery2
    b. cd ~/pigallery2
    c. nano docker-compose.yml
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
        d. docker-compose up -d OR docker compose up -d
        e. docker ps (Verify that your docker is running) OR http://rpi-ip:5000

3. Install and Configure Nginx as a Reverse 
    a. sudo apt-get update
    b. sudo apt-get install nginx
    c. sudo nano /etc/nginx/sites-available/pigallery2
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
4. Enable the Configuration
    a. sudo ln -s /etc/nginx/sites-available/pigallery2 /etc/nginx/sites-enabled/

5. Test Configuration: 
    a. sudo nginx -t

6. Restart Nginx
    a. sudo systemctl restart nginx 
    b. http://rpi-ip

7. Systemd Service for Docker Compose
    a. sudp nano /etc/systemd/system/pigallery2.service
        [Unit]
        Description=PiGallery2 Docker Compose Service
        Requires=docker.service
        After=docker.service

        [Service]
        WorkingDirectory=/home/aya/pigallery2
        ExecStart=/usr/bin/docker/docker-compise up -d
        Restart=always
        User=aya
        Group=aya
        Type=oneshot
        RemainAfterExit=true

        [Install]
        WantedBy=multi-user.target

8. Reload systemd to recognize the new added service
    a. sudo systemctl daemon-reload

9. Enable service to start when the Raspberry Pi boots
    a. sudo systemctl enable pigallery2

10. Start service
    a. sudo systemctl start pigallery2

11. Reboot Raspberry Pi
    a. sudo reboot

12. Verify if everything is working
    a. docker ps
    b. systemctl status pigallery2
    c. http://rpi-pi
```
