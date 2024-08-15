# oneclick-gpu-pv
Enable GPU-PV easily

##
GPU Partition is a new Microsoft GPU virtualization technology avaiable in latest Win10 and Win11. However it is not configurable in an easy way( no GUI avaiable). This repository provides scripts for both Windows and Linux to enable GPU-PV easily.

## For Windows 10+
Depending on your system local. If it is English based or zh-CN, just download 'win-gpu-pv.ps1' and execute with Administrator priviledge. Otherwise you should implement your own check label located in the script.

_This script was inspired by [dantmnf](https://gist.github.com/dantmnf/9bf9972c1ad49029cbdc2e40f1b7ac51)_

## For Ubuntu(tested on 20.04 and 22.04)
Downdload the 'ubuntu-gpu-pv.ps1' and 'dxgkrnl-dkms.zip' to Windows host folder. Execute 'ubuntu-gpu-pv.ps1' with Administrator. You must configured WSLg first. That is to say you must have a WSL being able to use the GPU. If you didn't have ssh key generated in your WSL distro, you'll be asked for password many times.

_This script was inspired by [OlfillasOdikno](https://gist.github.com/OlfillasOdikno/f87a4444f00984625558dad053255ace)_

![屏幕截图 2022-07-05 144851 PNG](https://user-images.githubusercontent.com/2093588/179666690-01e4252a-9c97-44ca-a5cf-5dc627fb471b.jpg)

Execute the following scripts to install Nvidia CUDA.
```
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-keyring_1.0-1_all.deb
sudo dpkg -i cuda-keyring_1.0-1_all.deb
sudo apt-get update
sudo apt-get -y install cuda
```

## VA-API video acceleration
If you want to leverage the latest WSL2 video acceleration VA-API. 
1. First update mesa to latest version(>=23.0).
2. `sudo apt install mesa-va-drivers vainfo`.
3. Replace the following environment variables in "/etc/profiles.d/d3d.sh".
```
export LIBVA_DRIVER_NAME=d3d12
export MESA_LOADER_DRIVER_OVERRIDE=vgem
```
Then add your user to group "video" and "render". Now you can type "vainfo --display drm" to checkc if hardware codecs are shown.

### Known issues
* The GNOME desktop will go black when restarting, current workaround is disable GPU-PV temporarily with "Remove-VMGpuPartitionAdapter $vmName" in Powershell and add it back after entering the GNOME desktop.
