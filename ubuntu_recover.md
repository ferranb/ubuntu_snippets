**Disclaimer: Follow these notes at your own risk. I'm not responsible for anything you run in your devices. It's up to you!**

As you probably know, most systems break at the worsts moments. In those moments, addrenaline runs high and, for me, referring to these notes is essential. These are the steps that have help me for years to get through such situations. Until today, I haven't needed to do anithing other that follow these steps.

I recommend always carring a USB drive with a Live Ubuntu for those special moments. I also have a copy of these commands in my `/boot` partition.

Of course, these are the commands for *my* system. They have to be adapted for any other system.

# Mounting /boot 

    mkdir -p /tmp/mnt/boot
    sudo mount /dev/nvme0n1p1 /tmp/mnt/boot
    cp /tmp/mnt/boot/ubuntu_recuperacio* /tmp
    sudo umount /tmp/mnt/boot
    rmdir /tmp/mnt/boot

# Mount /root 

    mkdir -p /tmp/mnt/root
    sudo mount /dev/nvme0n1p5 /tmp/mnt/root
    sudo mount /dev/nvme0n1p1 /tmp/mnt/root/boot
    sudo mount /dev/nvme0n1p2 /tmp/mnt/root/boot/efi

# Check if the `crypttab` is correct

This is sometimes necessary in some installations when it happens that this file isn't created.

    if ! grep "crypt01 UUID" /tmp/mnt/root/etc/crypttab > /dev/null
    then 
       echo echo crypt01 UUID=$(sudo blkid -s UUID -o value /dev/nvme0n1p4) none luks,discard\
       >> /tmp/mnt/root/etc/crypttab 
    fi

# Mounting LUKS 

Make sure `cryptsetup` is installed in the Live system. If not:

    sudo apt install cryptsetup

Once you're sure:

    sudo lsblk -oNAME,FSTYPE,UUID | grep LUKS
    sudo cryptsetup luksOpen /dev/nvme0n1p4 crypt01
    sudo lsblk -oNAME,FSTYPE,UUID 
    sudo mount /dev/mapper/crypt01 /tmp/mnt/root/dades

Just a message for my future self: next time, pick a shorter and simpler encryption key: your data isn't that important. Thanks.

# `chroot` your system
    
    sudo mount --bind /dev /tmp/mnt/root/dev
    sudo mount --bind /run/lvm /tmp/mnt/root/run/lvm
    sudo chroot /tmp/mnt/root
    mount -t proc proc /proc
    mount -t sysfs sys /sys
    mount -t devpts devpts /dev/pts

# Recreate initrams images

This usually isn't necessary, but on one a laptop I had, I mistakenly craeted a small `/boot` partition. When adding new kernels after an `apt upgrade`, it ran out of space. After that, the system wouldn't boot unless I selected an older kernel in GRUB.

    sudo update-initramfs -k all -c

To create only a specific kernel, you need to run:

    sudo update-initramfs -k x.x.x-x-x -c

To check the installed kernels (WARNING: if you are running on a Live system, make sure to do the `chroot` first):

    sudo apt list --installed | grep linux-image 

As an alternative, you can check the contents of `/boot`, but be aware that there might be some old garbage.

Also, be aware that runing `uname -r` will show the kernel verions of the Live system. It doesn't make sense to check the current kernel from withihn a chrooted system. Instead, you can check the default one in `/boot`.

# Install/Reinstall GRUB

    sudo grub-install /dev/sda
    sudo update-grub

# Check the UEFI boot 

    sudo efibootmgr
