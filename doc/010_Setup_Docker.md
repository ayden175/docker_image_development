# Docker

You need Docker Version > 19.03, it should be available from the OS repositories (Ubuntu: sudo apt install docker.io)

### Restart and convenience 

In order to be able to run docker commands without being root, you should add your user to the `docker` group (requires new login to take effect).
```bash
sudo adduser $(id -un) docker
```

In order to make sure that the kernel modules are properly loaded, you should restart your computer. 

Now you are ready to use docker!

# NVIDIA Docker (Optional)

**In case you have an nvidia graphics card with driver support installed**

You can proceed to install NVIDIA Docker to enable 3d acceleration for docker.

Note: This requires to have the current NVIDIA driver installed, ubuntu is not updating major versions, check your 
package manager for the latest main package of the driver. When this document was written (30.11.2018) the latest version
in the standard ubuntu package repositories was: nvidia-driver-390. You can look for more recent drivers (higher numbers) via `apt search nvidia-driver-*`.

**Please follow the installation procedure [here.](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker)**

On laptops with hybrid graphics cards, it is very likely that you need to set the nvidia card as default using `sudo prime-select nvidia` and reboot (this might affect battery life). 

**IMPORTANT:** In order to enable 3d acceleration, existing containers have to be re-created (which is explained [here](020_Usage.md#container-management)).



### Using a registry
If you want to use the image generated by the docker build from this repository, you can use the already build image from the registry server. A list of all available images can be found on [docker hub](https://hub.docker.com/u/developmentimage).

If you want to pull images from a private registry, you have to consider the following three prerequisites.

1. install the docker credential helper:
```bash
sudo apt install golang-docker-credential-helpers
```
(For ubuntu 16.04 and older this is not a package and has to be grabbed from https://github.com/docker/docker-credential-helpers/releases, install the bin in /usr/bin using the one containing "secretservice" in it's name.)

2. Set up docker to use the credential helper:
Open the file ~/.docker/config.json (or create it if it doesn't exist), and put the following content in it:
```json
{
        "credsStore": "secretservice"
}
```
3. Now login to the registry server:
```bash
docker login docker-reg.mydomain.de
```
Now you can pull images from the docker registry, e.g. by using docker pull.

The `settings.bash` file allows to set `DOCKER_REGISTRY_AUTOPULL` in order to automatically pull images, when `./exec.bash` is run.
