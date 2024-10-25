# Creating a Custom Execution Environment for AWX

This repository provides instructions and files to create a custom execution environment for AWX based on the tutorial by Ugur AKGUL. For detailed steps and explanations, refer to the [Medium tutorial](https://ugurakgul.medium.com/creating-a-custom-execution-environment-for-awx-2a976ab3432f).

## Prerequisites

- Docker installed on your local machine or server.
- WSL and Ubuntu on your windows machine.
- Access to AWX installation files (e.g., RPM package).
- Basic understanding of Docker and AWX configurations.
- Python3
- Ansible Builder

## Steps to Build the Docker Image

1. **Clone this Repository:**

   ```bash
   git clone <repository-url>
   cd <repository-name>
   ```

## Configuring The Dependencies

NOTE: If you want to use ansible-builder v3, you need to install Python3.9 on your system.

After installing docker and python3, you can install ansible-builder with the command:

   ```bash
   python3 -m pip install ansible-builder
   ```

Based on your python3 version, ansible-builder v1 or v3 will be installed. If you have python3.9 or upper ansible-builder v3 will be installed and if you have lower python3 version, ansible-builder v1 will be installed.

After you are done with the prerequisites, you can begin to work with the customization. The repository contains folders/files that can help you customize your image. Under builder/dependencies folder there are 3 different files with the following purpose:

- bindep.txt: Is used for RPM packages that needs to be installed on the image
- requirements.txt: Is used for python packages/libraries that needs to be installed on the image
- requirements.yml: Is used for ansible-galaxy modules that needs to be installed on the image

After modifying requirements files, you need to modify your main file which is execution-environment.yml.

You can edit these execution-environment.yml files based on your specific needs, and change it’s name to execution-environment.yml

## Creating The Execution Environment

After you modified the above files based on your need, you can create your EE with below with:

    ```bash
    ansible-builder build --tag <your_registry_name>/awx/ee:<your_base_ee_version>-custom --container-runtime docker --verbosity 3
    ```

This command will create your execution environment and it will take some time, let’s take a look at the options:

- tag is used for tagging the newly created docker image
<your_base_ee_version> is used here to know which image is your image based on
- container-runtime is docker, so our images will be docker images
- verbosity is just the configuration of the command about how verbose should it be.

NOTE: There is caching when creating an image, if your build fails, cached steps can be used again. But if you don’t want to use that cache, you can remove it with clearing the folder context, in your folder structure.

After you’ve successfully created your image, you can push the image to your image registry and create an execution environment in AWX.

To push your image:

    ```bash
    docker push <your_registry_name>/awx/ee:<your_base_ee_version>-custom
    ```