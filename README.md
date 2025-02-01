#ARM (aarch64), virt based emulator on extended Ubuntu 22.04 (jammy) cloud image

## How to:
### This section explains steps taken
Note: because I am not very "common" with emulation, especially with: qemu-system-aarch64 environments, I will drop all I found.

#### Download base cloud image
Source of base image:   
https://cloud-images.ubuntu.com/jammy/20250112/  
In my case: I download them to folder ./base_images   
wget https://cloud-images.ubuntu.com/jammy/20250112/jammy-server-cloudimg-arm64.tar.gz ./base_images/jammy-server-cloudimg-arm64.tar.gz 

#### Prepare the image
untar (unarchive) .img file from archive 
```
cd ./base_images  
tar -xvf ./jammy-server-cloudimg-arm64.tar.gz  
```
</br>
After first test I noticed with LightDM and XFCE4 environment with (optional: gcc and g++ cmake make automake) installed it takes about 3-5 Gigabytes. But beying a bit paranoid I resized image to 32G. I would still leave it at at least 16G. But I use 32G. In case we want some KDE or anything :).  

##### Resize the base image we will work with:
qemu-img resize  32G

## Run in qemu ARM64 environment (to be redacted)
qemu-system-aarch64 -m 8192 -cpu max -M virt -smp 6 -bios /usr/share/qemu-efi-aarch64/QEMU_EFI.fd -drive if=none,file=./jammy-server-cloudimg-arm64.img,id=hd0 -device v
irtio-blk-device,drive=hd0 -netdev user,id=net0,hostfwd=tcp::2222-:22,hostfwd=tcp::5901-:5900 -device virtio-net-device,netdev=net0 -drive if=virtio,file=cloud-init.iso,format=raw -device usb-ehci -device usb-kbd -device usb-mouse -devi
ce virtio-gpu-pci,xres=1366,yres=768 -nographic -display sdl,gl=on -vnc :0
