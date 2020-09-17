[![Build Status](https://travis-ci.org/anbox/anbox-modules.svg?branch=master)](https://travis-ci.org/anbox/anbox-modules)

# Legacy Anbox Kernel Modules

This repository contains a renamed binder kernel module necessary to run older
versions of the Anbox Android container runtime; specifically, this older
version creates the /dev/binder device.

While newer versions of anbox should be able to use the binder_linux module
included with newer kernels, some packaged versions of anbox expect
/dev/binder to exist instead of supporting binderfs.  While not an ideal
solution, the legacy binder_anbox module can be used in newer kernels with
these older versions of anbox.

# Install Instruction

You need to have `dkms` and linux-headers on your system. You can install them by
`sudo apt install dkms` or `sudo yum install dkms` (`dkms` is available in epel repo
for CentOS).

Package name for linux-headers varies on different distributions, e.g.
`linux-headers-generic` (Ubuntu), `linux-headers-amd64` (Debian),
`kernel-devel` (CentOS, Fedora), `kernel-default-devel` (openSUSE).


You can either run `./INSTALL.sh` script to automate the installation steps or follow them manually below:

* First install the configuration files:

  ```
  $ sudo cp anbox.conf /etc/modules-load.d/
  $ sudo cp 99-anbox.rules /lib/udev/rules.d/
  ```

* Then copy the module sources to `/usr/src/`:

  ```
  $ sudo cp -rT binder /usr/src/anbox-binder-1
  ```

* Finally use `dkms` to build and install:

  ```
  $ sudo dkms install anbox-binder/1
  ```

You can verify by loading these modules and checking the created devices:

```
$ sudo modprobe binder_anbox
$ lsmod | grep -e binder_anbox
$ ls -alh /dev/binder
```

You are expected to see output like:

```
binder_anbox          114688  0
crw-rw-rw- 1 root root 511,  0 Jun 19 16:30 /dev/binder
```
