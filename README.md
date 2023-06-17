# rtk-ubuntu20

```bash
sudo apt install build-essential git libssl-dev libelf-dev flex bison
```

```bash
sudo apt install libncurses5-dev libncursesw5-dev dwarves
```

Download kernel source and patch for that version:

```bash
wget https://mirrors.edge.kernel.org/pub/linux/kernel/v5.x/linux-5.4.143.tar.xz
wget https://mirrors.edge.kernel.org/pub/linux/kernel/projects/rt/5.4/older/patch-5.4.143-rt64.patch.xz
```

Unpack and apply patches:
```bash
tar -xf linux-5.4.143.tar.xz
cd linux-5.4.143
```
```bash
xzcat ../patch-5.4.143-rt64.patch.xz | patch -p1
```
copy the config file
```bash
cp /boot/config-5.4.0-152-generic .config
```
```bash
make oldconfig
```
Configure, and when asked for `Preemption Model select the Fully Preemptible Kernel. Accept the default value for the rest`:

NOW:
Edit the .config file and change `CONFIG_SYSTEM_TRUSTED_KEYS="debian/canonical-certs.pem"` to `CONFIG_SYSTEM_TRUSTED_KEYS=""`

Build kernel:
```bash
make -j8 deb-pkg
```
Various deb packages will appear in the home directories:

Install the generated packages.

```bash
sudo dpkg -i ../linux-headers-5.4.143-rt64_5.4.143-rt64-1_amd64.deb ../linux-image-5.4.143-rt64_5.4.143-rt64-1_amd64.deb ../linux-libc-dev_5.4.143-rt64-1_amd64.deb
```
Now update the grub and reboot the system:
```bash
sudo update-grub
sudo reboot now
```

After reboot you should be able to see something like this:

![image](https://github.com/AdityaKoranga/rtk-ubuntu20/assets/95766110/360e7deb-cc2f-4ef9-8218-4da56bfe3710)


