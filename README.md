# Locally Debugging NestJS Apps in Kubernetes

## Overview

This repository provides an example of setting up and debugging two NestJS applications (`nest-one` and `nest-two`) in a local Kubernetes environment. The goal is to replicate a production-like setup to ensure efficient debugging. This guide covers setting up Docker, Kubernetes manifests, and Skaffold to enable a streamlined debugging workflow.

## Prerequisites

To complete this project, you need the following tools installed on your local machine:

- [Docker Desktop](https://www.docker.com/products/docker-desktop) (with Kubernetes enabled)
- [Node.js](https://nodejs.org/) (version 22.6.0 recommended)
- [Nest CLI](https://docs.nestjs.com/cli/overview)
- [Skaffold](https://skaffold.dev/)
- [VS Code](https://code.visualstudio.com/) with [Google Cloud Code extension](https://marketplace.visualstudio.com/items?itemName=GoogleCloudTools.cloudcode)

## Setting Up the Project

1. Create a folder for your project and navigate into it:

    ```bash
    mkdir nestjs-k8s-debugging
    cd nestjs-k8s-debugging
    ```

2. Use the Nest CLI to create two NestJS applications:

    ```bash
    npx @nestjs/cli new nest-one
    npx @nestjs/cli new nest-two
    ```

3. Build both applications:

    ```bash
    cd nest-one
    npm run build
    cd ../nest-two
    npm run build
    cd ..
    ```

4. Add Dockerfiles to each application (`nest-one` and `nest-two`):

    ```dockerfile
    FROM node:22.6.0

    WORKDIR /usr/src/app

    COPY package*.json ./

    RUN npm install

    COPY . .

    CMD [ "npm", "run", "start:debug" ]
    ```

## Kubernetes Setup

1. Create a `k8s` folder to store your Kubernetes manifests:

    ```bash
    mkdir k8s
    ```

2. Add deployment and service manifests for each application (`nest-one` and `nest-two`) in the `k8s` folder.

3. Create an Ingress manifest (`ingress.yaml`) to expose both applications at different paths.

## Skaffold Configuration

1. Initialize Skaffold in the project root:

    ```bash
    skaffold init
    ```

2. Select the appropriate Dockerfiles for `nest-one` and `nest-two` to build the images.

3. Use the following Skaffold manifest (`skaffold.yaml`):

    ```yaml
    apiVersion: skaffold/v4beta11
    kind: Config
    metadata:
      name: nestjs-k-s-debugging
    build:
      artifacts:
        - image: nest-one
          context: nest-one
          docker:
            dockerfile: Dockerfile
        - image: nest-two
          context: nest-two
          docker:
            dockerfile: Dockerfile
    manifests:
      rawYaml:
        - k8s/ingress.yaml
        - k8s/nestone-deployment.yaml
        - k8s/nestone-service.yaml
        - k8s/nesttwo-deployment.yaml
        - k8s/nesttwo-service.yaml
    ```

## Debugging with VS Code

1. Open VS Code, press `Ctrl + Shift + P`, and select **Cloud Code: Debug on Kubernetes**.
2. Choose the **Docker Desktop** context when prompted.
3. Confirm the directory in the remote container as `/usr/src/app` for both applications.

## Testing

1. Place breakpoints in the services at the lines:

    - `return 'Service One!';`
    - `return 'Service Two!';`

2. In your browser, make requests to the following URLs to trigger the breakpoints:

    - [http://localhost/nestone](http://localhost/nestone)
    - [http://localhost/nesttwo](http://localhost/nesttwo)

3. The execution will pause at the breakpoints, allowing you to inspect the state of the application.

## Conclusion

By following this guide, you will have a local Kubernetes setup running two NestJS applications, allowing you to debug and test efficiently. This setup helps replicate a production-like environment, ensuring smoother application development and troubleshooting.