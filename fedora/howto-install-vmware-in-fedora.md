1. Download VMWare bundle from the official site.
2. Add execution privileges to that `.bundle` file.
3. Install the bundle:
```bash
sudo ./<bundle-file>
```
4. Once installation is complete, run VMWare player.
5. If it says that it wasn't able to find Kernel Headers for your kernel, install them using `dnf` from Fedora repo or manually from an RPM.
6. Try running again. If it gives you a popup saying: ***Before you can run VMware Workstation, several modules must be compiled and loaded into the running kernel.*** then click *Cancel*, go to the terminal and run the following command: 
```bash
vmware-modconfig --console --install-all
```
7. Try running VMWare player again and it should work this time.
8. *Bonus:*
If while patching VMPlayer to run OSX, you get the following error: ***cp: cannot stat ‘/usr/lib/vmware/lib/libvmwarebase.so.0/libvmwarebase.so.0’: No such file or directory*** then go to the terminal and run the following commands:
```bash
sudo ln -s /usr/lib/vmware/lib/libvmwarebase.so /usr/lib/vmware/lib/libvmwarebase.so.0
sudo ln -s /usr/lib/vmware/lib/libvmwarebase.so/libvmwarebase.so /usr/lib/vmware/lib/libvmwarebase.so/libvmwarebase.so.0
```
