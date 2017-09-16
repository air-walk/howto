# How to get Bluetooth working in Fedora

1. Go to Koji and find the repo for your Kernel, sample link [here](https://kojipkgs.fedoraproject.org/packages/kernel/4.7.4/200.fc24/x86_64/)

2. Download the following `.rpms` from there:
```
   kernel-core-4.7.4-200.fc24.x86_64.rpm
   kernel-headers-4.7.4-200.fc24.x86_64.rpm
   kernel-modules-4.7.4-200.fc24.x86_64.rpm
   kernel-modules-extra-4.7.4-200.fc24.x86_64.rpm
   kernel-4.7.4-200.fc24.x86_64.rpm
   kernel-devel-4.7.4-200.fc24.x86_64.rpm
```
3. Install all these packages using dnf:
```bash
   sudo dnf install ./kernel-core-4.7.4-200.fc24.x86_64.rpm ./kernel-headers-4.7.4-200.fc24.x86_64.rpm \
                    ./kernel-modules-4.7.4-200.fc24.x86_64.rpm ./kernel-modules-extra-4.7.4-200.fc24.x86_64.rpm \
                    ./kernel-4.7.4-200.fc24.x86_64.rpm ./kernel-devel-4.7.4-200.fc24.x86_64.rpm
```
4. Take a checkout of the linux kernel and switch to the branch which has your kernel:
```bash
   cd /home/aditya/workspaces/git-repos
   git clone git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git
   cd linux
   git tag -l | less
   git checkout v4.7
```
5. Setup the config inside the checkout of your kernel src and choose what all things your want compiled:
```bash
   cd ./drivers/bluetooth
   cp /boot/config-`uname -r`* .config
   # make menuconfig
```
6. Patch the `btusb.c` file with the following [4]:
```
   -       { USB_DEVICE(0x0cf3, 0x3004), .driver_info = BTUSB_ATH3012 },
   +       { USB_DEVICE(0x0cf3, 0x3004), .driver_info = BTUSB_QCA_ROME },
```
7. Create/edit the Makefile in this directory:
```
   vim Makefile
```
8. Insert the following snippet in that Makefile:
```bash
   obj-m = btusb.o
   KVERSION = $(shell uname -r)
   all:
     make -C /lib/modules/$(KVERSION)/build M=$(PWD) modules
   clean:
     make -C /lib/modules/$(KVERSION)/build M=$(PWD) clean
```
9. Compile the module:
```
   make
```
10. Copy the generated `btusb.ko` file to the relevant directory, remove the older file and generate the newer `.xz` file:
```
    sudo cp ./btusb.ko /lib/modules/4.7.4-200.fc24.x86_64/kernel/drivers/bluetooth/btusb.ko
    cd /lib/modules/4.7.4-200.fc24.x86_64/kernel/drivers/bluetooth/
    sudo rm btusb.ko.xz
    sudo xz btusb.ko
```
11. Reboot


### References:
* [1] https://kernelnewbies.org/KernelBuild
* [2] https://www.cyberciti.biz/tips/compiling-linux-kernel-module.html
* [3] https://forums.linuxmint.com/viewtopic.php?t=220336
* [4] https://bugzilla.kernel.org/attachment.cgi?id=197861
