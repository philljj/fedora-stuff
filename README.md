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
- https://github.com/philljj/vimrc/blob/master/vimrc

## Bashrc

Title function, and shorten terminal cmd prompt.

Add this to top of bashrc:

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
