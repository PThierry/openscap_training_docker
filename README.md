# SCAP training docker image

This Docker image handle a fully featured Debian 10 with an uptodate SCAP tooling (including OpenSCAP probes, SCAP-Security-guides for various targets and Scap-Workbench graphical tool.
The goal is to learn easily how to manipulate SCAP tool using this image as a security service for remote test machines such as virtual machines or physical hosts, for educational purpose

## Running Docker image

In order to execute scap-workbench, the docker image must be executed on a host with a graphical server installed and running. The local X server will be used in order as backend for the scap-workbench GUI.
You are free to handle the Docker container network depending on your needs, On of the way, in an educational example, is to share the host network namespace to easily intercact with other hosts.

A typical run of this container is the following:

   docker run --volume="$HOME/.Xauthority:/root/.Xauthority:rw" --env="DISPLAY" --net=host -it h2lab/scap_training


## Training

A typical training is based on the usage of various virtual machines supported by the SCAP guides:
- CentOS 6, 7, 8
- Fedora
- Debian 10
- Ubuntu 16.04, 18.04
etc.

Each of them must have the OpenSCAP probe installed and an SSH server listening.

When executing all these virtual machines in the same network (or at least all reachable by the container), we can start testing their configuration in comparison with the various security profiles defined in the SCAP-security-guide.

### 1. Trying to harden the target host


### 2. detect false positives or invalid checks (marked as 'error')


