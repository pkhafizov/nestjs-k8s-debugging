# How to Debug NestJS Micro-services Locally Inside Kubernetes with VS Code & Skaffold

## Overview

This repository provides an example of setting up and debugging two NestJS applications (`nest-one` and `nest-two`) in a local Kubernetes environment. The goal is to replicate a production-like setup to ensure efficient debugging. This guide covers setting up Docker and Kubernetes manifests to enable a streamlined debugging workflow.

## Project Structure

The repository is organized into the following folders and files:

- `nest-one/` - first sample NestJS service
- `nest-two/` - second sample service
- `k8s/` - Kubernetes manifests for both services
- `.vscode/launch.json` - VS Code debug settings for the Google Cloud Code extension

## Prerequisites

To complete this project, you need the following tools installed on your local machine:

- [Docker Desktop](https://www.docker.com/products/docker-desktop) (with Kubernetes enabled)
- [Node.js](https://nodejs.org/)
- [Nest CLI](https://docs.nestjs.com/cli/overview)
- [VS Code](https://code.visualstudio.com/) with [Google Cloud Code extension](https://marketplace.visualstudio.com/items?itemName=GoogleCloudTools.cloudcode)
- [Skaffold](https://skaffold.dev/)

## Setting Up the Project

1. Clone this repository and navigate into the project folder:

    ```bash
    git clone https://github.com/your-username/nestjs-k8s-debugging.git
    cd nestjs-k8s-debugging
    ```

2. Build both applications:

    ```bash
    cd nest-one
    npm install
    npm run build
    cd ../nest-two
    npm install
    npm run build
    cd ..
    ```


## Debugging with VS Code

1. Open VS Code, press `Ctrl + Shift + P`, and select **Cloud Code: Debug on Kubernetes**.
2. Choose the **Docker Desktop** context when prompted.
3. Confirm the directory in the remote container as `/app` for both applications.

## Testing

1. Place breakpoints in the services.

2. In your browser, make requests to the following URLs to trigger the breakpoints:

    - [http://localhost/nestone](http://localhost/nestone)
    - [http://localhost/nesttwo](http://localhost/nesttwo)

3. The execution will pause at the breakpoints, allowing you to inspect the state of the application.

## Conclusion

By following this guide, you will have a local Kubernetes setup running two NestJS applications, allowing you to debug and test efficiently. This setup helps replicate a production-like environment, ensuring smoother application development and troubleshooting.
