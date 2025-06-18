# How to Debug NestJS Micro-services Locally Inside Kubernetes with VS Code & skaffold

[![Build Status](https://img.shields.io/github/actions/workflow/status/pkhafizov/nestjs-k8s-debugging/ci.yml?branch=main)](https://github.com/pkhafizov/nestjs-k8s-debugging/actions) [![License](https://img.shields.io/github/license/pkhafizov/nestjs-k8s-debugging)](https://github.com/pkhafizov/nestjs-k8s-debugging/blob/main/LICENSE) [![Docker Pulls](https://img.shields.io/docker/pulls/pkhafizov/nest-one)](https://hub.docker.com/r/pkhafizov/nest-one)

A step-by-step guide to run and debug two NestJS micro-services inside a local Kubernetes cluster using Skaffold and VS Code.

## Table of Contents

* [Overview](#overview)
* [Prerequisites](#prerequisites)
* [Project Structure](#project-structure)
* [Setting Up the Project](#setting-up-the-project)
* [Debugging with VS Code](#debugging-with-vs-code)
* [Testing](#testing)

## Overview

Running services in a Kubernetes environment locally ensures parity between development and production, enabling you to catch configuration issues early. This repository demonstrates how to use Docker Desktop's Kubernetes, Skaffold, and VS Code to build, deploy, and attach a debugger to two example NestJS services.

## Prerequisites

| Tool                                | Minimum Version | Installation                                                                                                                                                     |
| ----------------------------------- | --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Docker Desktop (Kubernetes enabled) | ≥ 4.x           | [https://docs.docker.com/desktop/](https://docs.docker.com/desktop/)                                                                                             |
| Node.js                             | ≥ 18.x          | [https://nodejs.org/](https://nodejs.org/)                                                                                                                       |
| Nest CLI                            | ≥ 9.x           | `npm i -g @nestjs/cli`                                                                                                                                           |
| Skaffold                            | ≥ 2.x           | [https://skaffold.dev/](https://skaffold.dev/)                                                                                                                   |
| VS Code + Cloud Code Extension      | Latest          | [https://marketplace.visualstudio.com/items?itemName=GoogleCloudTools.cloudcode](https://marketplace.visualstudio.com/items?itemName=GoogleCloudTools.cloudcode) |

## Project Structure

| Path            | Description                                 |
| --------------- | ------------------------------------------- |
| `nest-one/`     | Sample NestJS service #1                    |
| `nest-two/`     | Sample NestJS service #2                    |
| `k8s/`          | Kubernetes manifests (Deployment, Service)  |
| `.vscode/`      | VS Code debug configurations for Cloud Code |
| `skaffold.yaml` | Skaffold pipeline definition                |

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

3. If your Kubernetes cluster doesn't have an Ingress controller installed, you can easily set one up using Helm. For example, to install NGINX Ingress Controller, you can run the following command:

    ```bash
    helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
    helm repo update
    helm install nginx-ingress ingress-nginx/ingress-nginx
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
