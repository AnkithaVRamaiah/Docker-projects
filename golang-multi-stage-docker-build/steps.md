# Multi Stage Docker Build

The main purpose of choosing a golang based applciation to demostrate this example is golang is a statically-typed programming language that does not require a runtime in the traditional sense. Unlike dynamically-typed languages like Python, Ruby, and JavaScript, which rely on a runtime environment to execute their code, Go compiles directly to machine code, which can then be executed directly by the operating system.

So the real advantage of multi stage docker build and distro less images can be understand with a drastic decrease in the Image size.

### Steps to Run a Calculator Using a Multistage Dockerfile

1. **Create and Launch an EC2 Instance**
   - Create an Ubuntu EC2 instance on AWS.
   - SSH into the EC2 instance.

2. **Follow Installation Steps from README.md**
   - Clone the repository from GitHub and follow the installation steps provided in the `README.md` file.

3. **Write a Dockerfile** (Code already present in GitHub)
   - In the repository, the Dockerfile is already provided. You do not need to write the Dockerfile from scratch.
   - It is a **multistage Dockerfile**, which means it uses different build stages to create a lightweight production image.
   - Ensure that the `Dockerfile` is present in the root directory of your project.

4. **Build the Docker Image**
   - Once the setup is done and the Dockerfile is in place, build the Docker image using the following command:
     - Replace `ankithav` with your Docker Hub username.
     ```bash
     docker build -t ankithav/image-name:latest .
     ```
   - You should see an output like this:
     ```
     Successfully built a371532791d3
     Successfully tagged ankithav/image-name:latest
     ```

5. **Run the Docker Container**
   - After successfully building the image, run the container with:
     ```bash
     docker run -it ankithav/image-name
     ```
   - The container will start, and you should see the following prompt:
     ```
     Enter any calculation (Example: 1 + 2 (or) 2 * 5 -> Please maintain spaces as shown in example):
     ```
   - At this point, you can enter mathematical calculations like `1 + 2` or `2 * 5`, and the calculator will perform the operation and display the result.

6. **Push the Docker Image to Docker Hub**
   - After confirming that the container works as expected, push the image to Docker Hub so others can use it.
     ```bash
     docker push ankithav/image-name
     ```
   - The push command will output the following:
     ```
     Using default tag: latest
     The push refers to repository [docker.io/ankithav/image-name]
     a3e00071145f: Pushed
     abfa678ae1a3: Pushed
     e1b6827ff1b8: Pushed
     687d50f2f6a6: Mounted from library/ubuntu
     latest: digest: sha256:99482c082538b0d693c36e1005c2255d0cd9c1f472c5007b8027b1976fa0870e size: 1155
     ```

### Final Outcome:
- After running the container, you can interact with the calculator by entering mathematical operations.
- The image is now available on Docker Hub and can be pulled by anyone using:
  ```bash
  docker pull ankithav/image-name
  ```
