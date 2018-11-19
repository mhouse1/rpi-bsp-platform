# rpi-bsp-platform
Simplifies fetching repos for building yocto distros for rpi

* Use Ubuntu version less than 18.04. Doesn't work on 18.10

To get all sources run
```
repo init -u https://github.com/lighht/rpi-bsp-platform
repo sync -j4
```

This following command installs all dependencies assuming that the system is not setup of development.
```
sudo apt install gawk wget git-core diffstat unzip texinfo gcc-multilib \
build-essential chrpath socat cpio python python3 python3-pip python3-pexpect \
xz-utils debianutils iputils-ping libsdl1.2-dev xterm \
autoconf libtool libglib2.0-dev libarchive-dev python-git \
sed cvs subversion coreutils texi2html docbook-utils python-pysqlite2 \
help2man make gcc g++ desktop-file-utils libgl1-mesa-dev libglu1-mesa-dev \
mercurial automake groff curl lzop asciidoc u-boot-tools dos2unix mtd-utils pv \
libncurses5 libncurses5-dev libncursesw5-dev libelf-dev zlib1g-dev
```
