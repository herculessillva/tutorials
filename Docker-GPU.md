# Tutorial Docker + GPU

## Install Docker
> [docker](https://www.docker.com/)

- Update packages list:
```sh
sudo apt update
```

- Install prerequisite packages:
```sh
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

- Add Docker GPG key to your system:
```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

- Add Docker repository:
```sh
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
```

- Install docker:
```sh
sudo apt install docker-ce
```

- Check docker service status:
```sh
sudo systemctl status docker
```

- Add user to the docker group:
```sh
sudo usermod -aG docker ${USER}
```

- Re-login:
```sh
su - ${USER}
```

- View your groups names:
```sh
groups
```

- Add user to the docker group:
```sh
sudo usermod -aG docker username
```

- Run docker `Hello-World`:
```sh
docker run hello-world
```

- Command output:
```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

## Install Nvidia Drivers

- Install prerequisite packages:
```sh
sudo apt install g++ freeglut3-dev build-essential libx11-dev libxmu-dev libxi-dev libglu1-mesa libglu1-mesa-dev
```

- Get the PPA repository driver:
```sh
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update
```

- Find recommended driver versions:
```sh
ubuntu-drivers devices
```

- Install nvidia driver with dependencies:
```sh
sudo apt install libnvidia-common-515 libnvidia-gl-515 nvidia-driver-515 -y
```


## Installing Nvidia Container Toolkit (nvidia-ctk)

> [nvidia-ctk](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)

- Configure the repository:

```sh
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list \
  && \
    sudo apt-get update
```

- Install the NVIDIA Container Toolkit packages:
```sh
sudo apt-get install -y nvidia-container-toolkit
```

## Configuration nvidia-ctk

- Configure the container runtime by using the nvidia-ctk command:
```sh
sudo nvidia-ctk runtime configure --runtime=docker
```

- Restart the Docker daemon:
```sh
sudo systemctl restart docker
```

## Run sample workload
- Run a sample CUDA container:

```sh
docker run --rm --runtime=nvidia --gpus all ubuntu nvidia-smi
```

- Command output:
```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 510.108.03   Driver Version: 510.108.03   CUDA Version: 11.6     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA GeForce ...  On   | 00000000:01:00.0  On |                  N/A |
| N/A   40C    P3    10W /  N/A |     69MiB /  4096MiB |     32%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
+-----------------------------------------------------------------------------+
```