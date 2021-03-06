# Image structure

The image generated by this project is composed of three main parts
compatible with the DE10-nano development board:

- A bootloader: [Das U-Boot](https://www.denx.de/wiki/U-Boot)
- A custom Linux kernel: built from [source provided by Intel](https://github.com/altera-opensource/linux-socfpga)
- A custom rootfs built by [Buildroot](https://buildroot.org/)

In addition the build process generates a cross compilation toolchain using
Buildroot to compile all of the above.

# Build process

## Vagrant

Mr. Fusion uses [Vagrant](https://www.vagrantup.com/) and
[Virtualbox](https://www.virtualbox.org/) to manage a virtual machine that
compiles the bootloader, kernel and rootfs used in the image.
Install both applications on your system, following the documentation of those
projects.

Vagrant makes it easy to provision a virtual machine using a manifest called
the [Vagrantfile](https://www.vagrantup.com/docs/vagrantfile). The Vagrantfile
for this project is located in the [build-vm folder](https://github.com/MiSTer-devel/mr-fusion/tree/master/build-vm).

Change the memory, number of processor cores, disk size and software versions
variables in the Vagrantfile if necessary. Do this before provisioning the
VM as changes to the build scripts are not synced automatically, only when you
re-provision the machine (which you can do at no additional cost, but it's a
manual task).

_NOTE: Don't forget to update the number of parallel compile jobs in the
build.sh script if you change the amount of processor cores._

Clone this repository on your machine and provision the virtual machine:

```
git clone https://github.com/MiSTer-devel/mr-fusion.git
cd mr-fusion/build-vm
vagrant up
```

Provisioning the virtual machine will take some time and download a few
gigabytes of source code.

## Building the components

Once the machine is up and running and has been succesfully provisioned, ssh
into it:

```
vagrant ssh
```

Inside the build-vm you can now build the necessary components with the all
in one `build.sh` shell script:

```
./build.sh
```

This will build the cross compilation toolchain, the rootfs, the bootloader
and Linux kernel.

## Generating the SD card image

Once that has successfully completed you can generate the SD card image by
executing the following command:

```
./create-sd-card-image.sh
```

If succesful, the resulting image can be found in the build-vm/images folder
on the host machine (/vagrant/images on the guest machine). It is ready to be
flashed to an SD card.
