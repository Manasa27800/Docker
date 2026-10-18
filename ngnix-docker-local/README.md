
# Nginx Web Server Using Docker Desktop

## Project Overview
This project demonstrates how to run an Nginx web server inside a Docker container on a local Windows machine using Docker Desktop.

## Technologies Used
- Docker Desktop
- Nginx
- Docker Hub
- Windows

## Prerequisites
- Docker Desktop installed and running
- Windows laptop
- Internet connection

## Implementation Steps

### Step 1: Verify Docker Installation

```bash
docker --version
docker ps
```
<img width="1429" height="290" alt="verify" src="https://github.com/user-attachments/assets/25628632-26a9-4140-a2f6-463cda4512c4" />

### Step 2: Pull the Nginx Image

Download the official Nginx image from Docker Hub.

```bash
docker pull nginx
```

### Step 3: Create and Run the Container

```bash
docker run -d --name my-nginx -p 80:80 nginx
```

**Command Explanation:**
- `docker run` - Creates and starts a container.
- `-d` - Runs the container in the background.
- `--name my-nginx` - Assigns a name to the container.
- `-p 80:80` - Maps port 80 on the host to port 80 inside the container.
- `nginx` - Specifies the image to use.

### Step 4: Access the Nginx Web Server

Open your browser and visit:

http://localhost:80
<img width="1864" height="723" alt="webserver" src="https://github.com/user-attachments/assets/ca82f63d-649c-4c4a-a1bd-7777206f6fa9" />

The default Nginx welcome page should appear.

### Step 5: Verify the Container

```bash
docker ps
```
<img width="1439" height="391" alt="verify container" src="https://github.com/user-attachments/assets/26ef1a38-7a93-4f84-b460-2c3c9fcb22c3" />

Verify that the container is running and port 80 is mapped correctly.

### Step 6: Stop and Remove the Container

Stop the container:

```bash
docker stop my-nginx
```

Remove the container:

```bash
docker rm my-nginx
```
<img width="1374" height="392" alt="stop" src="https://github.com/user-attachments/assets/e9fb9235-c1da-4a05-8aa8-c20029056a27" />

## Port Mapping Explained

Docker uses the following format:

`-p HOST_PORT:CONTAINER_PORT`

Example:

```bash
docker run -d --name my-nginx-8080 -p 8080:80 nginx
```

Access the server at:

http://localhost:8080

Here, port 8080 on the host machine maps to port 80 inside the container.

## Architecture

Windows Laptop → Docker Desktop → Nginx Container → Web Browser

## Key Learnings
- Learned how to pull Docker images from Docker Hub.
- Understood the difference between Docker images and containers.
- Practiced running and managing a container.
- Learned host-to-container port mapping.
- Ran a web server locally without an AWS EC2 instance.

## Local Hosting vs AWS EC2

| Local Docker | AWS EC2 |
|---|---|
| Runs on your laptop | Runs on a cloud server |
| Accessed using localhost | Can be accessed using a public IP or domain |
| No AWS Security Group required | Security Group controls inbound traffic |
| Useful for development and testing | Can host publicly accessible websites |

## Conclusion

This project demonstrates how Docker simplifies running an Nginx web server using a container. It provides hands-on experience with Docker images, containers, and port mapping before deploying applications to the cloud.


