## Sample Docker Project

A simple docker image which hosts the Big Star Collectibles example python flask app.

## Building the Docker image locally

Clone the repo and, vavigate to the directory which contains the `dockerfile` then execute the docker build command below.

    docker build . -t sample-docker-project:$(date +%s) --rm

Command breakdown:
-  **`docker build`** This is the Docker command to build an image from a Dockerfile. The image is created based on the instructions specified in the Dockerfile.

-  **`.`**  *The dot (`.`) at the end of the command specifies the build context, which is the directory containing the Dockerfile and any other files needed for the build. In this case, `.` means the current directory. Docker will look in the current directory for the Dockerfile and all the files it needs to include in the image.*

-  **`-t sample-docker-project:$(date +%s)`** *This flag tags the image with a name and a version (or tag). sample-docker-project is the name of the image. $(date +%s) is a shell command that gets the current Unix timestamp.*

-  **`--rm`**  *This flag tells Docker to remove intermediate containers after a successful build. During the build process, Docker creates intermediate containers using `--rm` ensures these are cleaned up automatically, helping to save space and avoid clutter.*

## Running the container
    docker run -d -p 5000:5000 --name sample-docker-container --read-only sample-docker-project:0.1

Command breakdown:
-  **`docker run`**  *This is the basic Docker command to create and start a new container from a specified image.*

-  **`-d`**  *This flag runs the container in detached mode, which means the container runs in the background.*

-  **`-p 5000:5000`**  *This flag publishes a container's port(s) to the host, port 5000 on the host machine is mapped to port 5000 in the container.*

-  **`--name sample-docker-container`**  *This option assigns a name to the container. In this case, the container will be named sample-docker-container.*

-  **`--read-only`**  *This flag mounts the container's filesystem as read-only. This means that no changes can be made to the filesystem inside the container. This is useful for security reasons, ensuring that the container cannot modify its own filesystem.*

-  **`sample-docker-project:0.1`**  *This is the name and tag of the Docker image from which the container will be created. sample-docker-project is the name of the image. 0.1 is the tag (or version) of the image.*

When the container is running, you should be able to access the python flask app at http://127.0.0.1:5000

## Docker Image CI Workflow

This repository contains a GitHub Actions workflow for building and pushing a Docker image to Docker Hub. The workflow is triggered whenever code is pushed to the `main` branch.

1. **Trigger**:
   - The workflow is triggered by pushes to the `main` branch and pull requests targeting `main`.

2. **Steps**:
   - **Checkout**: The workflow checks out the repository's code.
   - **Log in to Docker Hub**: It uses the `DOCKERHUB_USERNAME` and `DOCKERHUB_TOKEN` repo secrets to login via docker cli.
   - **Build the Docker image**: The image is built from the repository's code and tagged with the current timestamp.
   - **Push the Docker image**: The image is pushed to Docker Hub.