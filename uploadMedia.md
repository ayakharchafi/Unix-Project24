#### Uploading images to the server (PiGallery2)
``` cmd
1. Update docker-compose.yml to correctly map our host's image directly to/app/data/images
    a. ***
       volumes:
         - ./images:/app/data/images
    b. mkdir -p ./images
    c. sudo chmod -R 777 ./images
    d. cp -r /home/aya/Desktop/PiPic/* ./images/
    e. docker compose down
    f. docker compose up -d
    g. check web server
```
