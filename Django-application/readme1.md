To create a Docker file, build an image, and run a container to see your Django application, follow these s

### 2. **Build Docker Image**
   Open a terminal in the directory where your `Dockerfile` is located and build the Docker image with the following command:

   ```bash
   docker build -t my-django-app .
   ```

   This will create a Docker image named `my-django-app` using the current directory (`.`) as the context.

### 3. **Run the Docker Container**
   After the image is built successfully, run a container from it:

   ```bash
   docker run -p 8000:8000 my-django-app
   ```

   This will start the Django application in the container and bind port 8000 on your machine to port 8000 inside the container.

### 4. **Access the Application**
   Open a web browser and navigate to `http://Ipaddress:8000/demo/`. You should see your Django application running.
