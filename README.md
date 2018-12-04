# rpi-bsp-platform
Simplifies fetching repos for building yocto distros for rpi. The instructions were compiled from instructions that can be found in 
https://jumpnowtek.com/rpi/Raspberry-Pi-Systems-with-Yocto.html

*Use Ubuntu version less than 18.04. Doesn't work on 18.10*



This following command installs all dependencies assuming that the system is not setup of development.
```
sudo apt install gawk wget git-core diffstat unzip texinfo gcc-multilib \
build-essential chrpath socat cpio python python3 python3-pip python3-pexpect \
xz-utils debianutils iputils-ping libsdl1.2-dev xterm \
autoconf libtool libglib2.0-dev libarchive-dev python-git \
sed cvs subversion coreutils texi2html docbook-utils python-pysqlite2 \
help2man make gcc g++ desktop-file-utils libgl1-mesa-dev libglu1-mesa-dev \
mercurial automake groff curl lzop asciidoc u-boot-tools dos2unix mtd-utils pv \
libncurses5 libncurses5-dev libncursesw5-dev libelf-dev zlib1g-dev git repo
```

To get all sources run
```
repo init -u https://github.com/lighht/rpi-bsp-platform
repo sync -j4
```

Once all the dependencies are built, you need to setup the environment by sourcing the oe-init-build-env file. If your directory structure is different, then you need to apply those changes in build/conf/bblayers.conf file. 
```
source sources/poky/oe-init-build-env build
```
The above command will setup the environment variables and put you inside a directory called build.

Copy the files scripts/conf/local.conf and scripts/conf/bblayers.conf fomr this directory to build/conf/

Now you can start building the images and sdk.

Run 
```
bitbake -c do_populate_sdk qt5-image
```

Run the SDK installer that is created at build/tmp/deploy/sdk/poky-glibc-x86_64-qt5-image-cortexa7hf-neon-vfpv4-toolchain-2.5.1.sh

If you have also built the image to install on rpi run the following commands to flash image to sd card
```
export MACHINE=raspberrypi3 OETMP="rpi/build-rpi/tmp/"; 
 ./sources/meta-rpi/scripts/copy_boot.sh sda
./sources/meta-rpi/scripts/copy_rootfs.sh sda qt5
```
