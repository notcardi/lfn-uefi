# Obtaining the kernel source
To stay true with Maple Circuit's tutorial, and to stay up to date, we are going to use the kernel's git repository.
Therefore, we are going to have to create a working directory for everything we have.

``# mkdir LFN-UEFI``\
``# cd LFN-UEFI``\
Now that we have the directory, let's make this organised. This will make it a lot easier to manage when we have a lot more packages.

``# mkdir src``\
``# cd src``\
Now, finally we can clone the kernel.\
``# git clone https://github.com/torvalds/linux``\
``# cd linux``

Before you think ahead, do not run 'make menuconfig' just yet. As the menuconfig will base it off of the kernel configuration you currently have. The problem with this is that it probably has a lot of kernel modules. To keep our build simple, we want to avoid the kernel modules and have everything built into our kernel. Therefore, first we run:
``# make defconfig``\
This will make the default configuration for x86_64 cpus. If you are running on something like a Raspberry Pi, this will not work, check the doccumentations for that as this wiki is for x86_64 ONLY.
Now, we run:
``make menuconfig``\ 
This will bring up an ncurses menu of all the configuration options that there is. It is a very nice interactive way to configure the kernel compared to old methods.
# Configuring the kernel.
This really is where you can go off, you can really do anything you want. But, for optimal UEFI support, here's what i'd reccomend enabling.
``Default console loglevel = 7 (CONSOLE_LOGLEVEL_DEFAULT)``\
``Quiet console loglevel = 7 (CONSOLE_LOGLEVEL_QUIET)``\
``Message loglevel = 7 (MESSAGE_LOGLEVEL_DEFAULT)``\
These will just simply make the kernel display every message possible, so that if there is an error, you will know about it.\
``Cirrus driver for QEMU = y (DRM_CIRRUS_QEMU)``\
``EFI-Based Framebuffer = y (FB_EFI)``\
``Framebuffer Console Support = y (FRAMEBUFFER_CONSOLE)``\
These are the options to make the actual display work, as if you do not enable these, your kernel will infinitely hang (well, there will be no display).\
# Compiling & Running the kernel.
When you are ready, you can now compile your kernel.\
`make -j$(nproc)`

As you may of guessed, we are going to use QEMU as our virtual machine. To test our newly created kernel, try the following command:\
``qemu-system-x86_64 -bios /usr/share/ovmf/OVMF.fd -kernel arch/x86/boot/bzImage``\
(If you are not on Debian or a Debian-based distribution (ex. Mint, Ubuntu), the path to ``OVMF.FD`` may be different, or it may even be called ``OVMF.4M.FD``. You can use these ones too if nessecary.)


If you see a bunch of logs and what looks like a kernel panic, you have done everything correctly. The reason it panics is because we don't have anything to give the kernel. There is no init, or a device to give it, just the kernel.


Now that you've compiled the kernel, you'll probably want some basic utilities like a shell, and any other core utilities that you would use on a daily basis. For that, see:\
[Core Utilities](coreutils/COREUTILS.md)
