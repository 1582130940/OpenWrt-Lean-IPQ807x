Welcome to Lean's git source of OpenWrt and packages
=

How to build OpenWrt firmware.
-
Note:
--
1. DO **NOT** USE **root** USER FOR COMPILING!!!

2. Web admin panel default IP is 192.168.123.1 and default password is "password".

Let's start!
---
1. First, install Ubuntu x64.

2. Run `sudo apt update` in the terminal, and then run
    `
    sudo apt -y install \
    asciidoc antlr3 autoconf automake autopoint \
    binutils build-essential bzip2 \
    curl \
    device-tree-compiler \
    flex \
    g++-multilib gawk gcc-multilib gettext git git-core gperf \
    lib32gcc1 \
    libc6-dev-i386 \
    libelf-dev \
    libglib2.0-dev \
    libncurses5-dev \
    libtool \
    libssl-dev \
    libz-dev \
    msmtp \
    p7zip p7zip-full patch python2.7 python3 \
    qemu-utils \
    rsync \
    subversion swig \
    texinfo \
    uglifyjs unzip upx \
    wget \
    xmlto \
    zlib1g-dev
    `

3. Run `git clone https://github.com/1582130940/OpenWrt-AMD64` to clone the source code, and then `cd lede` to enter the directory

4. ```bash
   ./scripts/feeds update -a
   ./scripts/feeds install -a
   make menuconfig
   ```

5. Run `make -j16 download V=s` to download libraries and dependencies

6. Run `make -j16 V=s` (integer following -j is the thread count, single-thread is recommended for the first build) to start building your firmware.

This source code is promised to be compiled successfully.

You can use this source code freely, but please link this GitHub repository when redistributing. Thank you for your cooperation!
=

Rebuild:
```bash
cd lede
git pull
./scripts/feeds update -a && ./scripts/feeds install -a
make defconfig
make -j16 download
make -j$(($(nproc) + 1)) V=s
```

If reconfiguration is need:
```bash
rm -rf ./tmp && rm -rf .config
make menuconfig
make -j$(($(nproc) + 1)) V=s
```

Build result will be produced to `bin/targets` directory.

Special tips:
------
1. This source code doesn't contain any backdoors or close source applications that can monitor/capture your HTTPS traffic, SSL is the final castle of cyber security. Safety is what a firmware should achieve.

## Note: Addition Lean's private package source code in `./package/lean` directory. Use it under GPL v3.

## GPLv3 is compatible with more licenses than GPLv2: it allows you to make combinations with code that has specific kinds of additional requirements that are not in GPLv3 itself. Section 7 has more information about this, including the list of additional requirements that are permitted.
