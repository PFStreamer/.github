## Introduction
Perfect Streamer® is software for delivering digital streams of television channels via public Internet network anywhere in the world in Point-to-Point mode.

It uses proprietary protocol Perfect Stream that ensures reliable and stable video stream delivery without using expensive networks.

Also, standard transport protocols Pro-MPEG, SRT and RIST are supported. This allows to organize channels both via Perfect Streamer® and other software or hardware supporting the protocols.

Transcoder supporting Nvidia Encoder and Software CPU methods.

You can find our site [here](https://pstreamer.tv) or read [documentation](http://doc.pstreamer.tv/en/index.html).

## Installation

### Install to Redhat family  (el7, el8+)

Install pstreamer repo (el7):
```
$ sudo yum-config-manager --add-repo=http://repo.pstreamer.tv/pub/pstreamer/pstreamer.repo
```
or el8+:
```
$ sudo yum config-manager --add-repo=http://repo.pstreamer.tv/pub/pstreamer/pstreamer.repo
```

Install pstreamer:
```
$ sudo yum -y install pstreamer
```

Complete remove:
```
$ sudo yum -y remove pstreamer aksusbd
```

### Install to Debian family (Ubuntu etc.)

```
$ sudo wget http://repo.psts.info/pub/deb/dists/pstreamer/pstreamer.list -O /etc/apt/sources.list.d/pstreamer.list     
$ sudo apt-get update
$ sudo apt-get install pstreamer
```
Complete remove:
```
$ sudo apt-get remove pstreamer aksusbd
```
### After the installation

Files:
```
/usr/local/bin/pss - pstreamer binary file
/opt/pss/config/pss.properties - global options
/opt/pss/config/pss_default.json - default configuration file
/usr/lib/systemd/system/pss.service - systemd service file
/var/log/pss - directory log files location
```
After install new package make trial activation:
```
$ /opt/pss/tools/activate.sh
```
After install new package use default web access:
* port: 8808
* login: admin
* password: admin

Service manipulations:
```
$ systemctl stop pss
$ systemctl start pss
$ systemctl restart pss
$ systemctl status pss
```

Hasp package: **aksusbd**

Hasp services: **hasplmd aksusbd**

If have start problem see log:
```
$ tail -n 20 /var/log/pss/main.log
$ journalctl -u pss -n 20
```
Check installed binary:
```
$ pss -h
```

### Transcoders

Perfect Streamer transcoders installation.

Packages available:

* pstreamer-tcsw: transcoding using CPU (Software).
* pstreamer-tcnv: transcoding using NVidia GPU. pstreamer package only (full protected version).

The main requirement is GLIBC version >= 2.28.

Now it works on Alma Linux 8.9

#### RHEL 8+
1. Install pstreamer or pstreamer-demo.
2. Add repository and update the system (if have not done yet):
```
sudo dnf config-manager --add-repo https://developer.download.nvidia.com/compute/cuda/repos/rhel9/x86_64/cuda-rhel9.repo
sudo dnf clean all
sudo dnf update -y
reboot
```
3. NVidia Encoder.
Install Cuda:
```
sudo dnf -y install cuda-toolkit-12-5
```
Install driver (choose the option):

*Legacy*
```
sudo dnf -y module install nvidia-driver:latest-dkms
```
*New*
```
sudo dnf -y module install nvidia-driver:open-dkms
```
Reboot after installation:
```
reboot
```
Check the driver after reboot:
```
nvidia-smi
modprobe nvidia
sudo lsmod | grep nvidia
```
or
```
modprobe nouveau
sudo lsmod | grep nouveau
```
4. Install transcoder packages:
CPU Software method:
```
sudo dnf install -y pstreamer-tcsw
```
Nvidia GPU:
```
sudo dnf install -y pstreamer-tcnv
```
#### Ubuntu 22/24
1. Install pstreamer or pstreamer-demo.
2. NVidia Encoder.
Install Cuda toolkit version 12.5:
```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt-get update
sudo apt-get -y install cuda-toolkit-12-5
```
Install driver (choose the option):

legacy kernel module flavor:
```
sudo apt-get install -y cuda-drivers
```
or

open kernel module flavor:
```
sudo apt-get install -y nvidia-driver-555-open
sudo apt-get install -y cuda-drivers-555
```
Reboot after installation:
```
reboot
```
Check the driver after reboot:
```
nvidia-smi
```
CUDA bersion 12.5 is needed for transcoder. A different version of CUDA may already be installed in the system.
It is also possible to upgrade CUDA to a newer version when updating the system.
This does not interfere with the work, they do not need to be deleted, different versions of CUDA are installed in separate folders.

3. Install transcoder packages:
CPU Software method:
```
sudo apt-get install -y pstreamer-tcsw
```
Nvidia GPU:
```
sudo apt-get install -y pstreamer-tcnv
```
Transcoder packages will install files:

* /usr/local/bin/tcsw - SW (CPU) transcoder binary file.
* /usr/local/bin/tcnv - NVidia (GPU) transcoder binary file.
* /opt/pss/config/pss_tc_sw.properties - SW (CPU) transcoder starting config file.
* /opt/pss/config/pss_tc_nv.properties - NVidia (GPU) transcoder starting config file.

4. Check transcoder installed.

Transcoder packages installation leeds to pss service restart. You can check transcoder installation in the About section, where transcoder version will be shown.

#### Other Debian and RHEL based OS
To install pstreamer-tcnv transcoder on other OS, except Ubuntu 22/24 and RHEL 8+, use the configurator to select the required version of CUDA and driver on the Nvidia website:

https://developer.nvidia.com/cuda-toolkit-archive

When choosing each of the CUDA versions, information is available on which OS version it is suitable for. Supported architecture is x86_64. If you need an older version of the OS, check its support in older versions of CUDA. The main requirement for the OS is support for GLIBC version >= 2.28

Nvidia driver must be installed from the repository offered for your OS version, on the page of the selected CUDA version.

Support of Nvidia video cards for the pstreamer-tcnv transcoder functionality can be checked on the Nvidia website in Video Encode and Decode Support Matrix. This page contains a support matrix for decoder and encoder video formats, as well as other characteristics.
