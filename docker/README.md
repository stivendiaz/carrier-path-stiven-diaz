## Docker Fundamentals:

1.  Creating Docker images using Dockerfiles:

    - Example: Assume you have a simple Node.js application. Create a Dockerfile that includes instructions to build an image for the application:
      - Specify a base image with the desired Node.js version.
      - Copy the application code into the image.
      - Run any necessary commands, such as installing dependencies using `npm install`.
      - Set the startup command to run the application using `npm start`.
    - Guide users through the process of building the Docker image using the Docker CLI: `docker build -t my-app-image .`.
    - Show how to verify the newly created image using `docker images`.

    Practical:

    - Dockerfile:

    ```Dockerfile
    FROM node:14
    WORKDIR /app
    COPY . .
    RUN npm install
    CMD ["npm", "start"]
    ```

    - Command-line instructions:

      ```
      $ docker build -t my-app-image .
      ```

    - Expected output:
      ```
      Successfully built <image_id>
      Successfully tagged my-app-image:latest
      ```

2.  Building efficient and optimized Docker images:

    - Example: Suppose you have a Python application that requires specific packages. Optimize the Dockerfile to improve the build process and reduce the image size:
      - Use multi-stage builds to separate the build environment from the runtime environment, minimizing the image size.
      - Utilize the `.dockerignore` file to exclude unnecessary files and directories from being added to the image.
      - Use lightweight base images (e.g., Alpine Linux) instead of larger ones when applicable.
      - Avoid installing unnecessary packages or dependencies.
    - Demonstrate the build process with the updated Dockerfile and compare the image sizes before and after the optimizations.

    Practical:

    - Dockerfile (optimized):

    ```Dockerfile
    # Build stage
    FROM node:14 AS build
    WORKDIR /app
    COPY package.json package-lock.json ./
    RUN npm ci
    COPY . .
    RUN npm run build

    # Final stage
    FROM node:14-alpine
    WORKDIR /app
    COPY --from=build /app/dist ./dist
    COPY --from=build /app/node_modules ./node_modules
    CMD ["npm", "start"]
    ```

    - Command-line instructions:

      ```
      $ docker build -t my-app-image .
      ```

    - Expected output:
      ```
      Successfully built <image_id>
      Successfully tagged my-app-image:latest
      ```

3.  Container networking and communication:

    - Example: Create two Docker containers and showcase networking and communication between them:
      - Create a container for a web server (e.g., Nginx) and expose port 80.
      - Create a container for a backend API (e.g., Node.js) and link it to the web server container.
      - Demonstrate how the web server container can communicate with the backend API container by making HTTP requests to the linked container's hostname.
      - Show how to specify and configure network aliases for containers to simplify communication.

    Practical:

    - Command-line instructions:

    ```
    $ docker run -d --name web-server -p 80:80 nginx
    $ docker run -d --name api-server --link web-server:web my-api-image
    $ docker exec -it api-server sh
    $ curl http://web
    ```

    - Expected output:
      - The web server container runs in detached mode and is accessible on port 80 of the host machine.
      - The API server container runs in detached mode and is linked to the web server container.
      - Accessing the API server container's shell allows executing commands within the container.
      - The `curl` command from the API server container successfully makes an HTTP request to the linked web server container.

4.  Managing container data with volumes and bind mounts:

    - Example: Illustrate how to manage data in Docker containers using volumes and bind mounts:
      - Create a Docker volume and mount it to a container for persistent data storage.
      - Demonstrate how changes made to the data within the container are persisted across container restarts.
      - Show how to use bind mounts to map a directory on the host machine to a directory inside the container.
      - Modify files within the bind mount directory on the host and observe the changes being reflected inside the container.

    Practical:

        - Command-line instructions:

    ```
    $ docker volume create my-data-volume
    $ docker run -d --name data-container -v my-data-volume:/data my-image
    $ docker exec -it data-container sh
    $ touch /data/myfile.txt
    $ exit
    $ docker rm -f data-container
    $ docker run -d --name data-container -v $(pwd)/my-data:/data my-image
    $ ls my-data
    ```

    - Expected output:
      - A Docker volume named `my-data-volume` is created.
      - A container named `data-container` is created with the volume mounted at `/data`.
      - Creating a file `myfile.txt` within the container's `/data` directory.
      - Removing the container `data-container`.
      - Recreating the container `data-container` with a bind mount that maps the `my-data` directory on the host to `/data` within the container.
      - The `ls` command within the `my-data` directory on the host shows the existence of `myfile.txt`.

5.  Docker security best practices:
    - Example: Discuss best practices for enhancing Docker container security:
      - Build images from trusted sources and regularly update base images and dependencies.
      - Implement user isolation by running containers with non-root users whenever possible.
      - Limit container privileges by using appropriate security options and capabilities.
      - Employ resource restrictions to prevent container abuse and resource exhaustion.
      - Implement network security measures like container network segmentation and firewall rules.
      - Scan Docker images for vulnerabilities using security scanning tools.
      - Encrypt sensitive data within containers and during image transfer.
      - Regularly monitor and audit Docker containers for security breaches.
      - Use trusted base images from official repositories or trusted vendors.
      - Regularly update base images and dependencies with security patches.
      - Run containers with non-root users whenever possible, by specifying a user in the Dockerfile or using the `--user` flag during container execution.
      - Limit container privileges by using the `

## Docker compose fundamentals:

1. Writing Docker Compose YAML files:

   - Example: Assume you have a simple web application consisting of a frontend and a backend. Create a Docker Compose YAML file to define the services:
     ```yaml
     version: "3"
     services:
       frontend:
         build: ./frontend
         ports:
           - 80:80
       backend:
         build: ./backend
         ports:
           - 8080:8080
     ```

2. Defining and managing multi-container applications:

   - Dockerfile (frontend):

     ```Dockerfile
     FROM nginx:latest
     COPY ./app /usr/share/nginx/html
     ```

   - Dockerfile (backend):

     ```Dockerfile
     FROM node:14
     WORKDIR /app
     COPY package.json package-lock.json ./
     RUN npm ci
     COPY . .
     CMD ["npm", "start"]
     ```

   - Command-line instructions:

     ```
     $ docker-compose up
     ```

   - Expected output:
     - Docker Compose builds and starts the frontend and backend services defined in the Docker Compose YAML file.
     - Both services are accessible on the specified ports (e.g., frontend on port 80, backend on port 8080).
     - Logs from both services are displayed in the console.

3. Container orchestration and service dependencies:

   - Docker Compose YAML:

     ```yaml
     version: "3"
     services:
       frontend:
         build: ./frontend
         ports:
           - 80:80
       backend:
         build: ./backend
         ports:
           - 8080:8080
         depends_on:
           - database
       database:
         image: mysql:5.7
         environment:
           - MYSQL_ROOT_PASSWORD=mysecretpassword
           - MYSQL_DATABASE=mydatabase
     ```

   - Command-line instructions:

     ```
     $ docker-compose up -d
     ```

   - Expected output:
     - Docker Compose builds and starts the frontend, backend, and database services defined in the Docker Compose YAML file.
     - The backend service waits for the database service to be up and running before starting.
     - All services are detached and run in the background.
     - Logs from the services can be viewed using `docker-compose logs`.

4. Scaling and load balancing with Docker Compose:

   - Docker Compose YAML:

     ```yaml
     version: "3"
     services:
       frontend:
         build: ./frontend
         ports:
           - 80:80
         scale: 2
     ```

   - Command-line instructions:

     ```
     $ docker-compose up -d --scale frontend=2
     ```

   - Expected output:
     - Docker Compose scales the frontend service to two instances.
     - The frontend service is accessible on port 80, and load balancing is automatically applied between the instances.
     - Logs from the services can be viewed using `docker-compose logs`.

## Docker Networking and Storage

1. Network modes and their use cases:

   - Command-line instructions:

     ```
     $ docker network create my-network
     $ docker run -d --name container1 --network my-network nginx
     $ docker run -d --name container2 --network my-network nginx
     $ docker network ls
     ```

   - Expected output:
     - A Docker network named `my-network` is created.
     - Two containers, `container1` and `container2`, are created and connected to the `my-network` network.
     - The `docker network ls` command lists the available networks, including `my-network`.

2. Container-to-container communication within a network:

   - Command-line instructions:

     ```
     $ docker run -d --name backend --network my-network my-backend-image
     $ docker run -it --name frontend --network my-network my-frontend-image
     $ ping backend
     ```

   - Expected output:
     - A container named `backend` is created using the `my-backend-image` image and connected to the `my-network` network.
     - A container named `frontend` is created using the `my-frontend-image` image and connected to the `my-network` network.
     - The `ping backend` command executed inside the `frontend` container successfully communicates with the `backend` container.

3. Storage options in Docker: volumes, bind mounts, and storage drivers:

   - Command-line instructions:

     ```
     $ docker run -d --name container1 -v my-volume:/data my-image
     $ docker run -d --name container2 -v $(pwd)/host-dir:/data my-image
     $ docker volume create my-volume
     $ docker volume ls
     ```

   - Expected output:
     - A container named `container1` is created with a named volume `my-volume` mounted at `/data` inside the container.
     - A container named `container2` is created with a bind mount that maps the `host-dir` directory on the host machine to `/data` inside the container.
     - The `docker volume create` command creates a new named volume named `my-volume`.
     - The `docker volume ls` command lists the available volumes, including `my-volume`.

4. Configuring and optimizing Docker storage:

   - Command-line instructions:

     ```
     $ docker system prune -a --volumes
     $ docker info | grep "Storage Driver"
     ```

   - Expected output:
     - The `docker system prune -a --volumes` command cleans up all unused containers, networks, images, and volumes, including removing dangling volumes.
     - The `docker info | grep "Storage Driver"` command displays the currently configured storage driver for Docker.

## Docker Compose in Production

1. Deploying Docker Compose applications to production:

   - Command-line instructions:

     ```
     $ docker-compose -f docker-compose.prod.yml up -d
     ```

   - Expected output:
     - Docker Compose deploys the production version of the application defined in the `docker-compose.prod.yml` file.
     - All services are started in detached mode and run in the background.

2. Using environment variables and secrets in Docker Compose:

   - Docker Compose YAML (with environment variables):

     ```yaml
     version: "3"
     services:
       backend:
         build:
           context: .
           args:
             - DATABASE_URL=${DATABASE_URL}
         environment:
           - NODE_ENV=production
     ```

   - Command-line instructions:

     ```
     $ export DATABASE_URL=your_database_url
     $ docker-compose up -d
     ```

   - Expected output:
     - The `DATABASE_URL` environment variable is exported with the desired value.
     - Docker Compose uses the environment variable during the build process or runtime to configure the backend service.

3. Container orchestration with Docker Swarm and Kubernetes:

   - Docker Compose YAML (for Docker Swarm):

     ```yaml
     version: "3"
     services:
       frontend:
         image: my-frontend-image
         deploy:
           replicas: 3
           placement:
             constraints:
               - node.role == worker
     ```

   - Command-line instructions (for Docker Swarm):

     ```
     $ docker swarm init
     $ docker stack deploy -c docker-compose.yml my-app
     ```

   - Expected output:
     - Docker Swarm is initialized, and a swarm manager is created.
     - The `docker stack deploy` command deploys the application stack defined in the `docker-compose.yml` file with three replicas of the frontend service running on worker nodes.

4. Continuous integration and deployment with Docker and Docker Compose:

   - Example instructions:
     - Set up a CI/CD pipeline with a tool like Jenkins, GitLab CI/CD, or Travis CI.
     - Configure the pipeline to build the Docker images and push them to a container registry.
     - Use a Docker Compose file to define the services and dependencies of the application.
     - Deploy the application using the appropriate command-line instructions, such as `docker-compose up -d`, on the target environment.

5. Monitoring and troubleshooting Dockerized applications:

   - Command-line instructions:

     ```
     $ docker-compose logs -f
     $ docker stats
     $ docker exec -it container_name bash
     ```

   - Expected output:
     - The `docker-compose logs -f` command displays the logs of all services in real-time.
     - The `docker stats` command shows resource usage statistics of running containers.
     - The `docker exec -it container_name bash` command opens an interactive shell inside a running container for troubleshooting purposes.

## Dockerizing Different Types of Applications

1. Dockerizing web applications (frontend, backend, databases):

   - Dockerfile (frontend):

     ```Dockerfile
     FROM nginx:latest
     COPY ./dist /usr/share/nginx/html
     ```

   - Dockerfile (backend):

     ```Dockerfile
     FROM node:14
     WORKDIR /app
     COPY package.json package-lock.json ./
     RUN npm ci
     COPY . .
     CMD ["npm", "start"]
     ```

   - Command-line instructions:

     ```
     $ docker build -t my-frontend-image -f Dockerfile.frontend .
     $ docker build -t my-backend-image -f Dockerfile.backend .
     ```

   - Expected output:
     - Docker builds the frontend and backend images using the respective Dockerfiles.
     - The images are tagged as `my-frontend-image` and `my-backend-image`.

2. Containerizing microservices architecture:

   - Docker Compose YAML:

     ```yaml
     version: "3"
     services:
       frontend:
         image: my-frontend-image
         ports:
           - 80:80
       backend:
         image: my-backend-image
         ports:
           - 8080:8080
     ```

   - Command-line instructions:

     ```
     $ docker-compose up -d
     ```

   - Expected output:
     - Docker Compose starts the frontend and backend services using the specified images.
     - The services are detached and run in the background.
     - The frontend service is accessible on port 80, and the backend service is accessible on port 8080.

3. Dockerizing legacy applications:

   - Dockerfile (legacy application):

     ```Dockerfile
     FROM ubuntu:latest
     COPY legacy-app /app
     CMD ["/app/legacy-app"]
     ```

   - Command-line instructions:

     ```
     $ docker build -t my-legacy-app-image -f Dockerfile.legacy .
     $ docker run -d my-legacy-app-image
     ```

   - Expected output:
     - Docker builds the legacy application image using the specified Dockerfile.
     - The image is tagged as `my-legacy-app-image`.
     - A container is created using the `my-legacy-app-image` image and runs in detached mode.

4. Optimizing Docker images for different scenarios:

   - Dockerfile (optimized):

     ```Dockerfile
     FROM node:14 AS build
     WORKDIR /app
     COPY package.json package-lock.json ./
     RUN npm ci
     COPY . .
     RUN npm run build

     FROM nginx:latest
     COPY --from=build /app/dist /usr/share/nginx/html
     ```

   - Command-line instructions:

     ```
     $ docker build -t my-optimized-frontend-image -f Dockerfile.optimized .
     ```

   - Expected output:
     - Docker builds the optimized frontend image using the specified Dockerfile.
     - The image is tagged as `my-optimized-frontend-image`.

5. Design patterns and best practices for Docker containers:
   - Example instructions:
     - Use a .dockerignore file to exclude unnecessary files and directories from the Docker build context.
     - Leverage multi-stage builds to minimize the size of the final Docker image.
     - Use environment variables to configure containerized applications.
     - Implement health checks to monitor the status of containers.
     - Run containers with non-root users whenever possible.
     - Monitor container logs and performance metrics for troubleshooting and optimization.
