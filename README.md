# Fedora Stuff

Things I wished I'd written down if I have to wipe and
reinstall.

## Chrome

```sh
sudo dnf install fedora-workstation-repositories

sudo dnf config-manager --set-enabled google-chrome

sudo dnf install google-chrome-stable
```

### Chrome silliness

If chrome crashes, then won't launch again:

```
rm -rf ~/.config/google-chrome/Singleton*
```

- https://forums.fedoraforum.org/showthread.php?332678-Google-chrome-doesn-t-start&p=1883589#post1883589

If you can't update Chrome through the browser, then update
through dnf:

```
sudo dnf update google-chrome-stable
```

## Vim

Get full vim install, not just minimal:

```sh
sudo dnf install vim
```

### Colors

Get legacy color schemes:
- https://github.com/vim/colorschemes/tree/master/legacy_colors

```sh
git clone https://github.com/vim/colorschemes.git
cd colorschemes/legacy_colors/
mkdir -p ~/.vim/colors
cp elflord.vim ~/.vim/colors/
```

### vimrc

Get my vimrc:
- https://github.com/philljj/vimrc

## Bashrc

Handy things. Add these to top of bashrc:

- Cap man page column width at 70:
```sh
# cap man page column width at 70
export MANWIDTH=70
```

- Title function, and shorten terminal cmd prompt.
```sh
# Set title to arg passed, or to current dir.
# Attribution:
#   https://stackoverflow.com/a/75144186
function ttl() {
  if [ $# -eq 1 ]; then
    str=$1
  else
    str=$(pwd | awk -F '/' '{print $NF}')
  fi

  if [[ -z "$ORIG" ]]; then
    ORIG=$PS1
  fi
  TITLE="\[\e]2;$str\a\]"
  PS1=${ORIG}${TITLE}
}

# Shorten cmd prompt.
export PS1="[\u \W]\$"
```
## Codex CLI

```sh
curl -fsSL https://chatgpt.com/codex/install.sh | sh
```

## Github CLI

```sh
sudo dnf install gh
```

## Python3

```sh
sudo dnf install python3-jinja2
```

## Desktop background

Set scaled background image:

```sh
gsettings set org.gnome.desktop.background picture-options scaled
```

See options:
```sh
gsettings range org.gnome.desktop.background picture-options
```

References:

- https://askubuntu.com/a/1181775

## Git

Git aliases

```sh
git config --global alias.co checkout
git config --global alias.br branch
```

Git global user config

```sh
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```

Git credential store. This will save credentials plaintext:

```
git config --global credential.helper store
```

## Install Kernel Src Tree

```sh
sudo dnf install kernel-devel-$(uname -r)
```

Installs kernel src tree to:

```
$ls /usr/src/kernels/$(uname -r)
arch           drivers   ipc       Makefile.rhelver  samples     tools
block          fs        Kconfig   mm                scripts     usr
certs          include   kernel    Module.symvers    security    virt
crypto         init      lib       net               sound       vmlinux.h
Documentation  io_uring  Makefile  rust              System.map  vmlinux.id
```

## GNU toolchain

```
sudo dnf install autoconf
sudo dnf install automake
sudo dnf install libtool
sudo dnf install perl
sudo dnf install libasan
sudo dnf install libtsan
```

## Essential packages

```
sudo dnf install flex bison
sudo dnf install elfutils-libelf-devel
sudo dnf install pcre2grep
sudo dnf install patch
sudo dnf install valgrind
sudo dnf install openssl
sudo dnf install openssl-devel
sudo dnf install qemu
sudo dnf install dracut
sudo dnf install expect
sudo dnf install clang
sudo dnf install clang-devel
sudo dnf install glibc
sudo dnf install glibc-static
sudo dnf install busybox
sudo dnf install shellcheck
sudo dnf install cppcheck
sudo dnf install check
sudo dnf install check-devel
```

## Useful packages

```
sudo dnf install sensors
sudo dnf install nethogs
sudo dnf builddep systemd #systemd dependencies
sudo dnf install libgcrypt
sudo dnf install libgcrypt-devel
sudo dnf install libevent
sudo dnf install libevent-devel
sudo dnf install libpsl-devel # for building libcurl
sudo dnf install cmake # needed for west zephyr builds
sudo dnf install ninja-build # needed for west zephyr builds
pip3 install pyelftools # needed for west zephyr builds
sudo dnf install libpcap-devel # for testing lwip
```

## ssh server

install:

```
sudo dnf install openssh-server
```

start:

```
systemctl start sshd.service
```

## Useful tools

```
sudo dnf install meld
sudo dnf install lm_sensors
```


## gcc-arm-none-eabi-

```
sudo dnf install arm-none-eabi-gcc-cs
```

May also require:

```
sudo dnf install ncurses-compat-libs
sudo dnf install arm-none-eabi-newlib
```

## powerpc cross compile

```
sudo dnf install gcc-powerpc64-linux-gnu
```

## more qemu

```
sudo dnf install qemu-system-ppc virt-install libvirt-daemon-kvm
```

# Fedora Gaming

## DooM

nalika gzdoom seems to be well supported:
```sh
sudo dnf copr enable nalika/gzdoom
sudo dnf install gzdoom
```

## wine

```sh
sudo dnf install wine
```

Setup is easier on Fedora 44:
```sh
mkdir -p ~/games/example_game
export WINEPREFIX=$HOME/games/example_game
wineboot
winecfg
```

# FreeBSD stuff too

Build bsdkm driver:
```
#!/bin/sh
BSDKM_CFLAGS="-DWOLFSSL_BSDKM_VERBOSE_DEBUG -DWOLFSSL_BSDKM_FPU_DEBUG"

./configure --enable-freebsdkm --enable-freebsdkm-crypto-register \
  --enable-cryptonly --enable-crypttests \
  --enable-kernel-benchmarks --enable-all-crypto --enable-aesni \
  --enable-aesni-with-avx \
  CFLAGS="$BSDKM_CFLAGS" && make \
  || exit 1

file bsdkm/libwolfssl.ko && sudo kldload bsdkm/libwolfssl.ko || exit 1
```

Set date & time:
```
#!/bin/sh
sudo service ntpdate onestart
```
