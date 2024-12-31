### Steps for Volumes Project with Multistage Dockerfile

1. **Create and Launch an EC2 Instance**
   - Follow the same process as before to create an Ubuntu EC2 instance and SSH into it.

2. **Clone the GitHub Repository and Install Dependencies**
   - Clone the repository.
   - Follow the installation steps in the `README.md` file as provided in the repository.

3. **Prepare the Dockerfile**
   - Ensure that the Dockerfile is already present in the repository (as cloned in Step 2)
   - This project involves attaching a volume to persist the data (logs, calculations, etc.) even if the container is stopped or removed.

4. **Create a Docker Volume**
   - You can create a Docker volume that will be used to store logs or any other data that should persist across container restarts.
     ```bash
     docker volume create logs-volume
     ```

5. **Modify the Docker Run Command to Mount the Volume**
   - Now, you will run the container with the `--mount` option to attach the volume to the container. This ensures that data inside the container is saved to the volume.
   - Use the `--mount` flag to mount the volume (`logs-volume`) to a directory in the container where logs or other data will be stored.
     ```bash
     docker run -it --mount source=logs-volume,target=/app/logs ankithav/image-name
     ```
   - **Explanation**: 
     - `source=logs-volume` refers to the volume we created.
     - `target=/app/logs` is the directory inside the container where logs or data will be stored.

6. **Verify the Volume is Mounted**
   - Once the container is running, check the mounted volume by entering the container and inspecting the `/app/logs` directory.
     ```bash
     docker exec -it <container-id> bash
     cd /app/logs
     ls
     ```
   - Any log files or data written to `/app/logs` will be stored in the `logs-volume`.

7. **Stop and Remove the Container**
   - When you stop and remove the container, the data inside the container's filesystem is lost. However, the data inside the volume will persist.
     ```bash
     docker stop <container-id>
     docker rm <container-id>
     ```

8. **Verify the Volume Data Persistence**
   - To verify that the data is preserved, start a new container with the same volume attached:
     ```bash
     docker run -it --mount source=logs-volume,target=/app/logs ankithav/image-name
     ```
   - The logs and data should be available in the `/app/logs` directory, even after the container has been removed and recreated.

9. **Inspect the Volume**
   - You can inspect the details of the volume using:
     ```bash
     docker volume inspect logs-volume
     ```

10. **Push the Image to Docker Hub**
    - After testing the volume lifecycle, push the image to Docker Hub as previously described:
      ```bash
      docker push ankithav/image-name
      ```

### Additional Notes on Volume Lifecycle:
- **Creating a volume**: Volumes are created with `docker volume create`, and Docker manages the underlying storage.
- **Mounting a volume**: The `--mount` option allows you to attach the volume to a container. You can mount a volume to any directory inside the container.
- **Data Persistence**: Unlike bind mounts, which rely on host directories, Docker volumes are managed by Docker and stored in a location that Docker can control. The data persists even after the container is removed.
- **Volume Cleanup**: To clean up unused volumes, you can use:
  ```bash
  docker volume prune
  ```
  This removes all unused volumes, but be careful as it will delete data that is not being used by any container.

