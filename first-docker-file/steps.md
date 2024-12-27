INSTALL DOCKER
A very detailed instructions to install Docker are provide in the below link

https://docs.docker.com/get-docker/

Steps:

create an Ubuntu EC2 Instance on AWS, Login and run the below commands to install docker.

sudo apt update
sudo apt install docker.io -y
Start Docker and Grant Access
A very common mistake that many beginners do is, After they install docker using the sudo access, they miss the step to Start the Docker daemon and grant acess to the user they want to use to interact with docker and run docker commands.

Always ensure the docker daemon is up and running.

A easy way to verify your Docker installation is by running the below command

docker run hello-world
If the output says:

docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/create": dial unix /var/run/docker.sock: connect: permission denied.
See 'docker run --help'.
This can mean two things,

Docker deamon is not running.
Your user does not have access to run docker commands.
Start Docker daemon
You use the below command to verify if the docker daemon is actually started and Active

sudo systemctl status docker
If you notice that the docker daemon is not running, you can start the daemon using the below command

sudo systemctl start docker
Grant Access to your user to run docker commands
To grant access to your user to run the docker command, you should add the user to the Docker Linux group. Docker group is create by default when docker is installed.

sudo usermod -aG docker ubuntu
In the above command ubuntu is the name of the user, you can change the username appropriately.

NOTE: : You need to logout and login back for the changes to be reflected.

Docker is Installed, up and running 
Use the same command again, to verify that docker is up and running.

docker run hello-world
Output should look like:

....
....
Hello from Docker!
This message shows that your installation appears to be working correctly.
...
...

Clone this repository and move to example folder
git clone git@github.com:AnkithaVRamaiah/Docker-projects.git
cd  examples
Login to Docker [Create an account with https://hub.docker.com/]
docker login
Log in with your Docker ID or email address to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com/ to create one.
You can log in with your password or a Personal Access Token (PAT). Using a limited-scope PAT grants better security and is required for organizations using SSO. Learn more at https://docs.docker.com/go/access-tokens/

Username: ankithav
Password:
WARNING! Your password will be stored unencrypted in /home/ubuntu/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded

Build your first Docker Image
You need to change the username accoringly in the below command

docker build -t ankithav/my-first-docker-image:latest .
Output of the above command

Successfully built a371532791d3
Successfully tagged ankithav/my-first-docker-image:latest

Run your First Docker Container
docker run -it ankithav/my-first-docker-image
Output

Hello World
Push the Image to DockerHub and share it with the world
docker push ankithav/my-first-docker-image
Output

Using default tag: latest
The push refers to repository [docker.io/ankithav/my-first-docker-image]
a3e00071145f: Pushed
abfa678ae1a3: Pushed
e1b6827ff1b8: Pushed
687d50f2f6a6: Mounted from library/ubuntu
latest: digest: sha256:99482c082538b0d693c36e1005c2255d0cd9c1f472c5007b8027b1976fa0870e size: 1155

