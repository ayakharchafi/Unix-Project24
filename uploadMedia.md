## Uploading images to the server (PiGallery2)

This guide explains how to upload images to a server using PiGallery2 and Docker. Follow the steps carefully and you should be able to upload images successfully.

1. **Update docker-compose.yml to correctly map our host's image directory to /app/data/images**
   - The `docker-compose.yml` file is where Docker configurations for the project are stored. We will update this file to ensure that our local image directory is mapped to the Docker container's image directory.
   
   a. **Add the following line to the volumes section in the `docker-compose.yml` file**:
      ```yaml
      volumes:
        - ./images:/app/data/images
      ```
      This line tells Docker to map the local `./images` directory (on your host machine) to the `/app/data/images` directory in the Docker container. This way, any images placed in the local `./images` folder will be accessible by the PiGallery2 application inside the Docker container.

   b. **Create the images directory on your local machine if it doesn’t exist**:
      ```bash
      mkdir -p ./images
      ```
      This command creates the `./images` directory on your local machine if it doesn't already exist. The `-p` flag ensures that all parent directories are created if needed.

   c. **Change the permissions of the images directory to allow full access**:
      ```bash
      sudo chmod -R 777 ./images
      ```
      This command gives read, write, and execute permissions to the `./images` directory and all files inside it (`-R` stands for recursive). This is necessary to ensure that Docker and PiGallery2 can access and modify the files within this directory. *Note: Be cautious with `chmod 777` as it makes files accessible to everyone. In production environments, use more restricted permissions.*

   d. **Copy your images into the `./images` directory**:
      ```bash
      cp -r /home/aya/Desktop/PiPic/* ./images/
      ```
      This command copies all files from the `PiPic` folder on your desktop (`/home/aya/Desktop/PiPic/`) into the `./images` directory. The `-r` option ensures that if there are subdirectories inside `PiPic`, they will be copied as well.

   e. **Stop any running Docker containers**:
      ```bash
      docker compose down
      ```
      This command stops and removes the Docker containers, networks, and volumes defined in the `docker-compose.yml` file. It ensures that when you bring the container back up, it is using the updated configurations.

   f. **Start the Docker containers in detached mode**:
      ```bash
      docker compose up -d
      ```
      This command brings the Docker containers back up using the configurations in the `docker-compose.yml` file. The `-d` flag runs the containers in detached mode, meaning they will run in the background.

   g. **Check the web server**:
      Once the container is up, open your browser and navigate to the PiGallery2 web interface to verify that the images are correctly displayed. You should now be able to view the images you uploaded via the server.
