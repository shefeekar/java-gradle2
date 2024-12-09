# Java CI with Gradle and Docker Hub Integration

This repository demonstrates a **CI/CD pipeline** using **GitHub Actions** to build a Java project with **Gradle**, and then build and push a Docker image to **Docker Hub**. The pipeline automates the build and deployment process, ensuring quick and reliable updates.

---

## Features

- **Java Build Automation**: Leverages Gradle to compile and package Java applications.
- **Docker Multi-Platform Support**: Uses QEMU and Buildx to support cross-platform Docker image builds.
- **Seamless Docker Hub Integration**: Automates image pushing to Docker Hub.

---

## Workflow File: `Java CI with Gradle`

The GitHub Actions workflow, `Java CI with Gradle`, performs the following steps:

1. **Triggers**:
    - Runs on `push` and `pull_request` events targeting the `master` branch.
2. **Build Java Project**:
    - Sets up JDK 1.8.
    - Compiles and packages the Java project using Gradle.
3. **Docker Setup**:
    - Configures QEMU for multi-platform builds.
    - Sets up Docker Buildx for advanced Docker features.
4. **Docker Image Deployment**:
    - Logs into Docker Hub using repository secrets.
    - Builds the Docker image from the project and pushes it to Docker Hub.

---

## Prerequisites

1. **Secrets Configuration**:
Add the following secrets in your GitHub repository:
    - `DOCKERHUB_USERNAME`: Your Docker Hub username.
    - `DOCKERHUB_TOKEN`: Your Docker Hub password or access token.
2. **Docker Hub Repository**:
    - Ensure you have a repository created on Docker Hub for the image (e.g., `shefeekar/myrepo`).

---

## Workflow File

Here’s the GitHub Actions workflow used in this project:

```yaml
yaml
Copy code
# Java CI with Gradle and Docker Integration

name: Java CI with Gradle

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  build-java:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2

    - name: Set up JDK 1.8
      uses: actions/setup-java@v1
      with:
        java-version: 1.8

    - name: Grant execute permission for gradlew
      run: chmod +x gradlew

    - name: Build with Gradle
      run: ./gradlew build

    - name: Set up QEMU
      uses: docker/setup-qemu-action@v3

    - name: Set up Docker Buildx
      uses: docker/setup-buildx-action@v3

    - name: Login to Docker Hub
      uses: docker/login-action@v3
      with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

    - name: Build and push
      uses: docker/build-push-action@v5
      with:
          push: true
          tags: shefeekar/myrepo:latest

```

---

## How to Use

1. **Clone the Repository**:
    
    ```bash
    bash
    Copy code
    git clone https://github.com/shefeekar/java-gradle.git
    cd java-gradle
    
    ```
    
2. **Set Up Secrets**:
    - Add `DOCKERHUB_USERNAME` and `DOCKERHUB_TOKEN` in **Settings > Secrets and Variables > Actions**.
3. **Modify Dockerfile** (if needed):
Ensure the `Dockerfile` is correctly set up to containerize your application.
4. **Push Code**:
    - Commit and push your changes to the `master` branch. The workflow will automatically trigger.
5. **Check Docker Hub**:
    - Verify the new Docker image in your Docker Hub repository.

---

## Benefits

- **Automation**: Reduces manual effort in building and deploying applications.
- **Reliability**: Ensures consistent builds and deployments.
- **Multi-Platform Support**: Allows building images for different architectures.

---

## Contribution

Feel free to fork this repository, improve the workflow, and submit a pull request. Suggestions and issues are welcome!

---

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.
