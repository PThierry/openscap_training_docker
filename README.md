# SCAP training docker image

This Docker image handle a fully featured Debian 10 with an uptodate SCAP tooling (including OpenSCAP probes, SCAP-Security-guides for various targets and Scap-Workbench graphical tool.
The goal is to learn easily how to manipulate SCAP tool using this image as a security service for remote test machines such as virtual machines or physical hosts, for educational purpose

## Running Docker image

In order to execute scap-workbench, the docker image must be executed on a host with a graphical server installed and running. The local X server will be used in order as backend for the scap-workbench GUI.
You are free to handle the Docker container network depending on your needs, On of the way, in an educational example, is to share the host network namespace to easily intercact with other hosts.

A typical run of this container is the following:

>  docker run --volume="$HOME/.Xauthority:/home/oscap/.Xauthority:rw" --env="DISPLAY" --net=host -it philou8237/openscap_training

In order to get back reports results and other content to the host, it is possible to add another previously created shared directory:

>  docker run --volume="$HOME/.Xauthority:/home/oscap/.Xauthority:rw" --volume="$PWD/results:/home/oscap/results:rw" --env="DISPLAY" --net=host -it philou8237/openscap_training


## Training

A typical training is based on the usage of various virtual machines supported by the SCAP guides:
- CentOS 6, 7, 8
- Fedora
- Debian 10
- Ubuntu 16.04, 18.04
etc.

Each of them must have the OpenSCAP probe installed and an SSH server listening.

When executing all these virtual machines in the same network (or at least all reachable by the container), we can start testing their configuration in comparison with the various security profiles defined in the SCAP-security-guide.

### 1. First run, scanning a remote target

From the docker shell, execute:

> scap-workbench

and select the target Operating system reference.

When the overall ruleset is loaded, select the requested profile (there may have one or more profiles, depending on the support level. These profiles correspond to official hardening requirements such as PCI-DSS, ANSSI Technical guides, NIST-SP800 and so on.

Select the remote target to analyse by configurating the SSH connection. You must access a valid remote user account on the target. The target must also have OpenSCAP probes installed (yum install openscap, apt install libopenscap8, and so on).

Run the profile compliance test by clicking on "SCAN".

The results appears on the main window.


Example with remote CentOS8/Profile PCI_DSS.
If the report doesn't terminate properly, this may happen on a timeout on the "Verify and Correct File Permissions with RPM". This may happen if the target virtual machine is not powerful enough and generate a timeout for the probe. In this case:

   * increase the target power
   * create a custom profile from the PCI_DSS one (top right button "Customize"), and select the above rule and other RPM-based software hashes in the rules list and save the newly created profile with another name.


> INFO: remember to select the "fetch remote ressources" cell, at it is requested for remediation part.

### 2. Trying to harden the target manualy

Based on the results, try to harden the target. Missing packages have to be installed, missing configuration entries have to be upgraded/completed.

Launch back the scan while some hardening has been done to check that the modification are validated by the tool.

### 3. Use remediation scripts

Remediate to lots of non-conformity may be quite long and error-prone. The more target there are, the longer the remediation time is.

To automatize remediation, remediation scripts are built in the /usr/local/share/xml/... directory.
There is three types of remediation:

   * bash remediation, based on basic bash scripting
   * Ansible remediation, based on ansible playbook

Let's try bash-based remediation.

First, clear any previous report results, then:

   * select "fetch remote ressources"
   * select "bash remediation" in the bottom left remediation mode, but without selecting auto remediation
   * run again the report

When the security analysis is finished:

   * save the report in HTML & XCCDF (XML) format in the backed results directory
   * save the bash remediation script in the backed results directory

When you take a look to the remediation script, all failed rules for which a remediation script exists are concatenated into a single remediation script.

Copy this script to the target host and execute it with administrative rights.

When finished, rerun the analysis and compare with the previous results.

