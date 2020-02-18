# Development Containers: Azure CLI, Docker, Kubernetes, and Helm

This is a sample project that enables us to provide environment that can use Azure CLI, Docker, kubectl, and helm using **[VS Code Remote - Containers](https://aka.ms/vscode-remote/containers)** extension. This project provide bash environment with these tools already install and ready to go. Especially, it is useful for Windows 10 users to setup Linux based Bash environment with VSCode. 

> **Note:** If you're following the quick start, you can jump to the [Things to try](#things-to-try) section. 

## Setting up the development container

Follow these steps to open this sample in a container:

1. Install [Docker for Windows](https://docs.docker.com/docker-for-windows/install/). Then configure settings > General > Expose daemon on tcp://localhost:2375 without TLS on the docker for windows.  

2. If this is your first time using a development container, please follow the [getting started steps](https://aka.ms/vscode-remote/containers/getting-started).

3. To use this repository, you can either open a locally cloned copy of the code:

   - Clone this repository to your local filesystem.
   - Press <kbd>F1</kbd> and select the **Remote-Containers: Open Folder in Container...** command.
   - Select the cloned copy of this folder, wait for the container to start, and try things out!

4. Open the integrated terminal on VSCode. You will see the bash that can run Azure CLI, Docker, kubectl, and helm.

