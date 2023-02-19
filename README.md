# rpi-bsp-platform
Simplifies fetching repos for building yocto distros for rpi. The instructions were compiled from instructions that can be found in 
https://jumpnowtek.com/rpi/Raspberry-Pi-Systems-with-Yocto.html

Tested on Ubuntu 22.04.1 64-bit



This following command installs all dependencies assuming that the system is not setup of development.
```
apt install gawk wget diffstat unzip texinfo gcc-multilib build-essential chrpath socat cpio python2 python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping libsdl1.2-dev xterm autoconf libtool libglib2.0-dev libarchive-dev sed cvs subversion coreutils texi2html docbook-utils help2man make gcc g++ desktop-file-utils libgl1-mesa-dev libglu1-mesa-dev mercurial automake groff curl lzop asciidoc u-boot-tools dos2unix mtd-utils pv libncurses5 libncurses5-dev libncursesw5-dev libelf-dev zlib1g-dev
```

To get all sources run
```
repo init -u https://github.com/mhouse1/rpi-bsp-platform
repo sync --current-branch --no-clone-bundle
```

Once all the dependencies are built, you need to setup the environment by sourcing the oe-init-build-env file. If your directory structure is different, then you need to apply those changes in build/conf/bblayers.conf file. 
```
source sources/poky/oe-init-build-env build
```
The above command will setup the environment variables and put you inside a directory called build.

Copy the files scripts/conf/local.conf and scripts/conf/bblayers.conf from this directory to build/conf/

If you want to customize the splash screen, you need to edit the psplash_git.bb recipe found in sources/poky/meta/recipes-core/psplash and add a patch to edit color. The edited files are found in the psplash directory of this repo. Do a git diff to see the changes. Also add a psplash-poky-img.png of whatever splash image you want to see.  

Now you can start building the images

Run 
```
bitbake qt5-image
```
Before you start building the toolchain, we have to remove qtwebkit. I have not found any elegant way to work. So for now, forcibly
Delete the line "{USE_RUBY} \" from the file  sources/meta-qt5/recipes-qt/packagegroups/packagegroup-qt5-toolchain-target.bb
Then, build the toolchain.
```
bitbake meta-toolchain-qt5
```

Run the SDK installer that is created at build/tmp/deploy/sdk/poky-glibc-x86_64-meta-toolchain-qt5-cortexa7hf-neon-vfpv4-toolchain-2.5.1.sh

If you have also built the image to install on rpi run the following commands to flash image to sd card
```
export MACHINE=raspberrypi3 OETMP="rpi/build-rpi/tmp/"; 
 ./sources/meta-rpi/scripts/copy_boot.sh sda
./sources/meta-rpi/scripts/copy_rootfs.sh sda qt5
```
