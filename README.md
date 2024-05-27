# Arch Linux kernel custom config

## Build it yourself

**First make sure you have these modules installed on you system**

```bash
sudo pacman -S --needed xmlto kmod inetutils bc libelf git cpio perl tar xz
```

**Download the source**

Download stable release from https://kernel.org

The current version of this config uses v6.9.2

**Decompress**

Change `A.B.C` to the kernel's version numbers.

```bash
unxz --verbose /path/to/linux-A.B.C.tar.xz && \
tar --extract --verbose --file ./linux-A.B.C.tar
```

**cd into the kernel directory and configure or build it**

```bash
cd ./linux-A.B.C
make mrproper
cp /path/to/this/repo/.config ./
```

**If you want to configure yourself, do it now else SKIP THIS**

```bash
make xconfig
```

**Build time!**

```bash
make -j4 # change 4 to your number of CPUs

# when done
sudo make modules_install

# copy kernel to the /boot directory
sudo cp ./arch/x86/boot/bzImage /boot/vmlinuz-linux-ABC
# you can name vmlinuz-whatever-you_like
```

**If you have changed the `vmlinuz-linux-ABC` to something else change it
in the preset file, then copy the preset file to mkinitcpio.d/**

```bash
sudo cp ./linux-692.preset /etc/mkinitcpio.d/

# then create initramfs
sudo mkinitcpio --preset linux-692 # or whatever you named it

# then add your custom kernel to grub menu entry
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

**Reboot and enjoy**

## Things to note

This is configured only for my device, you may tweak it for yourself. And it
does NOT have any AMD, Ethernet, Joystick, Guest support modules. I REMOVED THEM.
