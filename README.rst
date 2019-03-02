ZynthianOS
===========

A `Raspberry Pi <http://www.raspberrypi.org/>`_ distribution to to run `Zynthian: The Open Synth Platform <http://zynthian.org/>`_ out of the box. This repository contains the source script to generate the distribution out of an existing `RealtimePi <https://github.com/guysoft/RealtimePi>`_ distro image. `You can download a built image here <http://unofficialpi.org/Distros/ZynthianOS/>`_

Where to get it?
----------------

Official mirror is `here <http://unofficialpi.org/Distros/ZynthianOS>`_


How to use it?
--------------

#. Unzip the image and install it to an SD card `like any other Raspberry Pi image <https://www.raspberrypi.org/documentation/installation/installing-images/README.md>`_
#. Configure your WiFi by editing ``realtimepi-network.txt`` or ``realtimepi-wpa-supplicant.txt`` on the root of the flashed card when using it like a flash drive
#. Boot the Pi from the SD card
# TODO: Add here what to do
#. If needed Log into your Pi via SSH (it is located at ``realtimepi.local`` `if your computer supports bonjour <https://learn.adafruit.com/bonjour-zeroconf-networking-for-windows-and-linux/overview>`_ or the IP address assigned by your router), default username is "pi", default password is "raspberry", change the password using the ``passwd`` command and expand the filesystem of the SD card through the corresponding option when running ``sudo raspi-config``.

Requirements
------------
* Raspberrypi 1, 2 or 3
* 2A power supply


Features
--------

* Zynthian out of the box, with a realtime kernel

Developing
----------

Requirements
~~~~~~~~~~~~

#. `qemu-arm-static <http://packages.debian.org/sid/qemu-user-static>`_
#. `CustomPiOS <https://github.com/guysoft/CustomPiOS>`_
#. Downloaded `RealtimePi <http://unofficialpi.org/Distros/RealtimePi/>`_ image.
#. root privileges for chroot
#. Bash
#. realpath
#. sudo (the script itself calls it, running as root without sudo won't work)

Build Raspbian / Debian / Ubuntu
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

ZynthianOS can be built from Debian, Ubuntu, Raspbian.
Build requires about 11 GB of free space available.
You can build it by issuing the following commands::

    sudo apt-get install realpath p7zip-full qemu-user-static
    
    git clone https://github.com/guysoft/CustomPiOS.git
    git clone https://github.com/guysoft/ZynthianOS.git
    cd ZynthianOS/src/image
    # If you want realtime kernel use this url:
    wget http://unofficialpi.org/Distros/RealtimePi/2017-12-20_2017-11-29-realtimepi-stretch-lite-0.2.zip
    
    # If you want a normal stretch lite kernel
    # wget -c --trust-server-names 'https://downloads.raspberrypi.org/raspbian_lite_latest'
    
    cd ..
    ../../CustomPiOS/src/update-custompios-paths
    sudo modprobe loop
    sudo bash -x ./build_dist
    
Building CustomPiOS Variants
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

CustomPiOS supports building variants, which are builds with changes from the main release build. An example and other variants are available in the folder ``src/variants/example``.

To build a variant use::

    sudo bash -x ./build_dist [Variant]
    
Building Using Vagrant
~~~~~~~~~~~~~~~~~~~~~~
There is a vagrant machine configuration to let build ZynthianOS in case your build environment behaves differently. Unless you do extra configuration, vagrant must run as root to have nfs folder sync working.

To use it::

    sudo apt-get install vagrant nfs-kernel-server
    sudo vagrant plugin install vagrant-nfs_guest
    sudo modprobe nfs
    cd CustomPiOS/src/vagrant
    sudo vagrant up

After provisioning the machine, its also possible to run a nightly build which updates from devel using::

    cd CustomPiOS/src/vagrant
    run_vagrant_build.sh
    
To build a variant on the machine simply run::

    cd ZynthianOS/src/vagrant
    run_vagrant_build.sh [Variant]

Usage
~~~~~

#. If needed, override existing config settings by creating a new file ``src/config.local``. You can override all settings found in ``src/config``. If you need to override the path to the Raspbian image to use for building OctoPi, override the path to be used in ``ZIP_IMG``. By default, the most recent file matching ``*-raspbian.zip`` found in ``src/image`` will be used.
#. If you want to include extra soundfonts in the image, you have to copy them to ``src/modules/zynthianos/filesystem/soundfonts``. You have to organize your soundfonts in 3 subdirs, by format: SF2, SFZ, GIG
#. Run ``src/build_dist`` as root.
#. The final image will be created in ``src/workspace``

Code contribution would be appreciated!
