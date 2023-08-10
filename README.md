# Connect Your Nvidia GPU to Jetson AGX Xavier
It is an instruction on how to use the PCI-e(Peripheral Component Interconnect Express) slot on Jetson AGX Xavier to connect your Nvidia GPU

## Prerequisites:
- The JetPack version must be newer than JetPack 5.0<br>
- Supported GPUs please refer to this link: [Nvidia Driver Info Pages](https://www.nvidia.com/Download/driverResults.aspx/210317/en-us/)<br>
- When setting up hardware connections, it's important to ensure the proper power supply for your GPU by using appropriate PCI-e power cables and an external power source. Connect your GPU securely to the designated PCI-e slot on the Jetson AGX Xavier. Currently, the HDMI cable should be connected to the Xavier, not the GPU.

## Install the Display Driver:
The Nvidia Linux-Aarch64 Display Drivers are suitable for AGX Xavier. Let's install the nvidia-driver-535 for an example.
```
sudo apt update
sudo apt install nvidia-driver-535 nvidia-dkms-535
```
During the installation, you may see the error message:
```
dpkg: error processing archive /var/cache/apt/archives/libnvidia-gl-535_535.86.0
5-0ubuntu0.20.04.2_arm64.deb (--unpack):
 trying to overwrite '/usr/share/glvnd/egl_vendor.d/10_nvidia.json', which is al
so in package nvidia-l4t-3d-core 35.2.1-20230124153320
dpkg-deb: error: paste subprocess was killed by signal (Broken pipe)
```
The errors you're seeing are due to dpkg (Debian package management system) trying to install a package that contains files that are already provided by another installed package. In this specific case, you're trying to install NVIDIA packages and they conflict with the already installed nvidia-l4t-* packages. To resolve this, you can force dpkg to overwrite the conflicting files.
```
sudo dpkg -i --force-overwrite /var/cache/apt/archives/libnvidia-compute-535_535.86.05-0ubuntu0.20.04.2_arm64.deb
sudo dpkg -i --force-overwrite /var/cache/apt/archives/libnvidia-common-535*.deb
sudo dpkg -i --force-overwrite /var/cache/apt/archives/libnvidia-gl-535_535.86.05-0ubuntu0.20.04.2_arm64.deb
sudo dpkg -i --force-overwrite /var/cache/apt/archives/libnvidia-extra-535_535.86.05-0ubuntu0.20.04.2_arm64.deb
sudo dpkg -i --force-overwrite /var/cache/apt/archives/nvidia-utils-535_535.86.05-0ubuntu0.20.04.2_arm64.deb
```
Then you can try to fix the broken dependencies and resume the installation:
```
sudo apt --fix-broken install
```
**Congratulations!** You have the display driver installed.

## After the Installation:
Please do a full system reboot. Move the HDMI connection from Xavier to your GPU. After the reboot, the integrated GPU on Xavier will stop working.<br><br>
You may use this command to check the running status of your GPU:
```
nvidia-smi
```

Enjoy!
