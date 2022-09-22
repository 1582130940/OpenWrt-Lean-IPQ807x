# 欢迎来到 Lean 的 LEDE 源码仓库

如何编译 LEDE 固件 [How to build LEDE firmware](./README_EN.md)

## 注意

1. **不要用 root 用户进行编译**
2. 默认登陆IP 192.168.123.1 密码 password

## 编译命令

1. 首先安装 Linux 系统，推荐 Debian 11 或 Ubuntu LTS

2. 安装编译依赖

   ```bash
   sudo apt update -y
   sudo apt full-upgrade -y
   sudo apt install -y \
   ack antlr3 aria2 asciidoc autoconf automake autopoint \
   binutils bison build-essential bzip2 \
   ccache cmake cpio curl \
   device-tree-compiler \
   fastjar flex \
   g++-multilib gawk gettext gcc-multilib git gperf \
   haveged help2man \
   intltool \
   libc6-dev-i386 \
   libelf-dev \
   libglib2.0-dev libgmp3-dev 、
   libltdl-dev \
   libmpc-dev libmpfr-dev \
   libncurses5-dev libncursesw5-dev \
   libpython3-dev \
   libreadline-dev \
   libssl-dev \
   libtool \
   lrzsz \
   mkisofs msmtp \
   nano ninja-build \
   p7zip p7zip-full patch pkgconf python2.7 python3 python3-pip \
   qemu-utils \
   rsync \
   scons squashfs-tools subversion swig \
   texinfo \
   uglifyjs unzip upx-ucl \
   vim \
   wget \
   xmlto xxd \
   zlib1g-dev
   ```

3. 下载源代码，更新 feeds 并选择配置

   ```bash
   git clone https://github.com/1582130940/OpenWrt-AMD64
   cd OpenWrt-AMD64
   ./scripts/feeds update -a
   ./scripts/feeds install -a
   make menuconfig
   ```

4. 下载 dl 库，编译固件
（-j 是线程数，第一次编译推荐单线程）

   ```bash
   make download -j16
   make V=s -j1
   ```

本套代码保证肯定可以编译成功。里面包括了 R23 所有源代码，包括 IPK。

二次编译：

```bash
cd OpenWrt-AMD64
git pull
./scripts/feeds update -a
./scripts/feeds install -a
make defconfig
make download -j16
make V=s -j$(nproc)
```

如果需要重新配置：

```bash
rm -rf ./tmp && rm -rf .config
make menuconfig
make V=s -j$(nproc)
```

编译完成后输出路径：bin/targets

### 使用 WSL/WSL2 进行编译

由于 WSL 的 PATH 中包含带有空格的 Windows 路径，有可能导致编译失败，请在 `make` 前加上：

```bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

### macOS 原生系统进行编译

1. 在 AppStore 中安装 Xcode

2. 安装 Homebrew：

   ```bash
   /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   ```

3. 使用 Homebrew 安装工具链、依赖与基础软件包:

   ```bash
   brew unlink awk
   brew install coreutils diffutils findutils gawk gnu-getopt gnu-tar grep make ncurses pkg-config quilt wget xz
   brew install gcc@11
   ```

4. 输入以下命令，添加到系统环境变量中：

   ```bash
   echo 'export PATH="/usr/local/opt/coreutils/libexec/gnubin:$PATH"' >> ~/.bashrc
   echo 'export PATH="/usr/local/opt/findutils/libexec/gnubin:$PATH"' >> ~/.bashrc
   echo 'export PATH="/usr/local/opt/gnu-getopt/bin:$PATH"' >> ~/.bashrc
   echo 'export PATH="/usr/local/opt/gnu-tar/libexec/gnubin:$PATH"' >> ~/.bashrc
   echo 'export PATH="/usr/local/opt/grep/libexec/gnubin:$PATH"' >> ~/.bashrc
   echo 'export PATH="/usr/local/opt/gnu-sed/libexec/gnubin:$PATH"' >> ~/.bashrc
   echo 'export PATH="/usr/local/opt/make/libexec/gnubin:$PATH"' >> ~/.bashrc
   ```

5. 重新加载 shell 启动文件 `source ~/.bashrc`，输入 `bash` 进入 bash shell，就可以和 Linux 一样正常编译

## 特别提示

1. 源代码中绝不含任何后门和可以监控或者劫持你的 HTTPS 的闭源软件， SSL 安全是互联网最后的壁垒。安全干净才是固件应该做到的。
