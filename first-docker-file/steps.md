# Docker Installation and Usage Guide

A very detailed guide to install Docker can be found [here](https://docs.docker.com/get-docker/).

## Steps:

### 1. Create an Ubuntu EC2 Instance on AWS
1. Launch an Ubuntu EC2 instance.
2. SSH into the instance.

### 2. Install Docker
Run the following commands to install Docker:
```bash
sudo apt update
sudo apt install docker.io -y
```

### 3. Start Docker and Grant Access
A common mistake beginners make is forgetting to:
1. Start the Docker daemon.
2. Grant access to the user to interact with Docker and run commands.

Always ensure the Docker daemon is running.

#### Verify Docker Installation
Run the following command:
```bash
docker run hello-world
```

If you encounter this error:
```bash
docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/create": dial unix /var/run/docker.sock: connect: permission denied.
See 'docker run --help'.
```
It means:
1. The Docker daemon is not running.
2. Your user does not have access to run Docker commands.

#### Start Docker Daemon
To verify if the Docker daemon is running:
```bash
sudo systemctl status docker
```

If it is not running, start it:
```bash
sudo systemctl start docker
```

#### Grant Access to Your User
Add the user to the Docker Linux group:
```bash
sudo usermod -aG docker ubuntu
```
Replace `ubuntu` with the appropriate username.

**Note:** You need to logout and login back for the changes to take effect.

#### Verify Installation Again
Run:
```bash
docker run hello-world
```
Expected output:
```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

### 4. Clone the Repository and Move to Example Folder
```bash
git clone git@github.com:AnkithaVRamaiah/Docker-projects.git
cd examples
```

### 5. Login to Docker Hub
Create an account at [Docker Hub](https://hub.docker.com/), then login:
```bash
docker login
```
Log in with your Docker ID or email address.

Example:
```bash
Username: ankithav
Password:
```
Output:
```
WARNING! Your password will be stored unencrypted in /home/ubuntu/.docker/config.json.
Configure a credential helper to remove this warning. See https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```

### 6. Build Your First Docker Image
Replace `ankithav` with your username:
```bash
docker build -t ankithav/my-first-docker-image:latest .
```
Output:
```
Successfully built a371532791d3
Successfully tagged ankithav/my-first-docker-image:latest
```

### 7. Run Your First Docker Container
```bash
docker run -it ankithav/my-first-docker-image
```
Output:
```
Hello World
```

### 8. Push the Image to Docker Hub
```bash
docker push ankithav/my-first-docker-image
```
Output:
```
Using default tag: latest
The push refers to repository [docker.io/ankithav/my-first-docker-image]
a3e00071145f: Pushed
abfa678ae1a3: Pushed
e1b6827ff1b8: Pushed
687d50f2f6a6: Mounted from library/ubuntu
latest: digest: sha256:99482c082538b0d693c36e1005c2255d0cd9c1f472c5007b8027b1976fa0870e size: 1155
