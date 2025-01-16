# Dockerized Automated Build_and_Push Project for a Static Website

A simple project demonstrating how to package and serve a static HTML site (downloaded from [TooPlates](https://www.tooplates.com/)) with **Nginx** in a Docker container. It will also automate the Docker image build and push process to Docker Hub using a **Bash script** and **Cron**.

---

## Table of Contents

1. [Overview](#overview)
2. [Project Structure](#project-structure)
3. [Creating the Dockerfile](#creating-the-dockerfile)
4. [Building the Docker Image](#building-the-docker-image)
5. [Running the Container](#running-the-container)
6. [Automating with a Bash Script](#automating-with-a-bash-script)
7. [Setting Up Cron](#setting-up-cron)
8. [Credits and License](#credits-and-license)

---

## Overview

- **Goal**: Serve a static website (downloaded from [TooPlates](https://www.tooplates.com/)) via **Nginx** in a Docker container.  
- **Key Concepts**:  
  - Creating a **Dockerfile**  
  - Using **Nginx** to serve static content  
  - Basic **Docker** commands (`build`, `run`, `push`)  
  - **Bash scripting** for Docker image automation  
  - **Cron** scheduling for periodic tasks

This project is a great starting point for anyone learning **Docker** or looking for a quick way to host a static site.

---

## Project Structure

static-site:version_no/ ├── .gitignore ├── Dockerfile ├── build-and-push.sh ├── README.md └── site/
├── index.html ├── about.html ├── css/ ├── js/ └── images/


- `site/`: All the HTML, CSS, and JavaScript files from [TooPlates](https://www.tooplates.com/).  
- `Dockerfile`: Defines the container environment (using the official `nginx:latest` image).  
- `build-and-push.sh`: A Bash script that builds the Docker image, logs into Docker Hub, and pushes the image.  
- `.dockerignore`: Helps exclude unnecessary files from the build context.  
- `README.md`: Documentation for this project (you’re reading it now!).

---

## Creating the Dockerfile

1. **Create** a file named **`Dockerfile`** in the root of your project (`my-static-site/`).  
2. **Add** the following content:

    ```dockerfile
    FROM nginx:latest
    RUN rm -rf /usr/share/nginx/html/*
    COPY site/ /usr/share/nginx/html
    EXPOSE 80
    CMD ["nginx", "-g", "daemon off;"]
    ```

### Explanation

- **FROM nginx:latest**: Uses the official Nginx image as a base.  
- **RUN rm -rf /usr/share/nginx/html/**:(Optional)  Removes default Nginx web content.  
- **COPY site/ /usr/share/nginx/html**: Copies your local `site/` files into the container.  
- **EXPOSE 80**: Lets Docker know the container listens on port 80.  
- **CMD ["nginx", "-g", "daemon off;"]**: Runs Nginx in the foreground so the container stays alive.

---

## Building the Docker Image

1. **Open a terminal** in your project directory (`my-static-site/`).  
2. **Build** the Docker image:

    ```bash
    docker build -t static-site:version_no .
    ```

   - `-t static-site:version_no` names your image `my-static-website` with version number.  
   - The final `.` points Docker to the current directory for the Dockerfile.

---

## Running the Container

Use the command below to run the container and map port **8080** on your machine to port **80** in the container:

```bash
docker run -d -p 8080:80 --name static-site static-site:version_no

-d: Runs in detached mode.
-p 8080:80: Forwards port 8080 on the host to port 80 inside the container.
--name static-site: Names the container “static-site.”
Now, open your browser to http://localhost:8080 and you should see the TooPlates-based site.

Automating with a Bash Script
The file build-and-push.sh shows how you can script the build-and-push process:
