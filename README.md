# Docker react setup in local

## Introduction

Before we begin, there are a few prerequisites that you should have installed on your machine.

1. Docker
2. Node.js

### Notes

If you don't already have Docker installed, you can download it from the [`Docker website`](https://www.docker.com/).
If you don't have them installed, you can download them from the ['Node.js website'](https://nodejs.org/en/download/package-manager/current).

To verify that Docker is installed, open your terminal and run the below command:

```bash
docker --version
```

This will display the version of Docker installed on your machine.<br/>
If Docker is not installed, you will see an error message.

To verify that Node.js and npm are installed, open your terminal and run the below command:

```bash
node --version
```

This will display the version of Node.js installed on your machine. If Node.js is not installed, you will see an error message.

In this guide, we will walk you through the process of deploying a React web application with Docker in local. We'll start by creating a new React application using `create-react-app` and then we will create a `Dockerfile` for our application. We will then build a Docker image and run a Docker container to test our application.

## **Create a React Application**

The first step is to create a new React application using `create-react-app` command. This command will set up a new React project with some default configuration files, dependencies, and a basic application structure.

```bash
npx create-react-app react-docker-setup
cd react-docker-setup
```

![reactapp](https://media.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fl41v7t9p9bsyun0dxfz0.png)

To run this react application execute the below command in your terminal:

```bash
npm run start
```

![](https://media.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fjxlw35wmf7y490ie4kfz.png)

## **Create a Dockerfile**

The next step is to create a Dockerfile for our React application. A Dockerfile is a text file that contains instructions for building a Docker image.

In your terminal, navigate to the root directory of your React application and create a new file named `Dockerfile`.

Open the `Dockerfile` in your code editor and add the following contents:

```bash
# Use an Node runtime as a parent image
FROM node:alpine

# Set the working directory to /app
WORKDIR /app

# Copy the package.json and package-lock.json to the working directory
COPY ./package*.json ./

# Install the dependencies
RUN npm install

# Copy the remaining application files to the working directory
COPY . .

# Build the application
RUN npm run build

# Expose port 3000 for the application
EXPOSE 3000

# Start the application
CMD [ "npm", "run", "start" ]
```

![](image-2.png)

## Building a Docker Image

Now that we have a Dockerfile for our React application, we can build a Docker image.

Create a new file named `.dockerignore` in the root directory of your React application and add the following line:

```bash
node_modules
```

In your terminal, navigate to the root directory of your React application and run the following command:

```bash
docker build -t react-application .
```

This command builds a Docker image with the tag `react-application` using the `.` to specify the build context as the current directory.

The build process may take a few minutes to complete, depending on the size of your application and the speed of your internet connection.

Once the build is complete, you can see image after type below command:

```bash
Docker image ls
```

!['command image'](https://media.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fga9pwc1elsznt6ywg3d4.png)

## Running the Docker Container

Now that we have a Docker image for our React application, we can run a Docker container.

In your terminal, run the below command to start a Docker container based on the image we just built:

```bash
docker run -d -p 3000:3000 react-application
```

This command starts a Docker container and maps port 3000 from the container to port 3000 on your local machine. The `react-application` parameter specifies the name of the Docker image that we want to run.

With the `-d` flag, the command returns the ID of the container once it's started.

You can also use the `docker ps` command to see a list of running containers, including the ID of the container running your React application.

![](https://media.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F4p5jx5r9pak9otyi59f0.png)

Once the container is running, you should be able to view your React application by navigating to [`http://localhost:3000`](http://localhost:3000) in your web browser.

![as](https://media.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fw7u307bjkzvv1nlz9bsn.png)

Congratulations! You have now successfully deployed your React web application with Docker.

To stop the container, you can use the `docker stop` command followed by the container ID

```bash
docker stop <container_id>
docker stop c8872dfba1b0
```

## Conclusion

In this guide, we have learned how to deploy a React web application with Docker. We started by creating a Dockerfile to package our application code and dependencies into a Docker image. Then, we built and ran the Docker container to verify that our application is running correctly.

We also covered how to bind the container port to the host port to make the application accessible from the outside. We discussed how to run the container in detached mode
