# Debian Bookwom Post install

## install bash completion
sudo apt-get install bash-completion


## Install flatpack
https://flatpak.org/setup/Debian

$ apt install flatpak

$ apt install gnome-software-plugin-flatpak

$ flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo

## Install 88x2bu Drivers
https://github.com/cilynx/rtl88x2bu
$ sudo apt-get install git dkms

$ git clone https://github.com/cilynx/rtl88x2bu

$ cd rtl88x2bu/

# DKMS as above
$ VER=$(sed -n 's/\PACKAGE_VERSION="\(.*\)"/\1/p' dkms.conf)

$ sudo rsync -rvhP ./ /usr/src/rtl88x2bu-${VER}

$ sudo dkms add -m rtl88x2bu -v ${VER}

$ sudo dkms build -m rtl88x2bu -v ${VER} # Takes ~3-minutes on a 3B+

$ sudo dkms install -m rtl88x2bu -v ${VER}

### Blacklist kernel drivers

$ echo "blacklist rtw88_8822bu" | sudo tee /etc/modprobe.d/rtw8822bu.conf
