# Debian 12 Bookwom Post install

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

#### DKMS as above
$ VER=$(sed -n 's/\PACKAGE_VERSION="\(.*\)"/\1/p' dkms.conf)

$ sudo rsync -rvhP ./ /usr/src/rtl88x2bu-${VER}

$ sudo dkms add -m rtl88x2bu -v ${VER}

$ sudo dkms build -m rtl88x2bu -v ${VER} # Takes ~3-minutes on a 3B+

$ sudo dkms install -m rtl88x2bu -v ${VER}

### Blacklist kernel drivers

$ echo "blacklist rtw88_8822bu" | sudo tee /etc/modprobe.d/rtw8822bu.conf

## Install Realtek RTL8192eu
https://github.com/clnhub/rtl8192eu-linux

$ sudo apt install linux-headers-generic build-essential dkms git

$ git clone https://github.com/clnhub/rtl8192eu-linux

$ cd rtl8192eu-linux

$ sudo ./install_wifi.sh

$ echo "blacklist rtl8xxxu" >> ./blacklist-rtl8xxxu.conf

$ sudo mv ./blacklist-rtl8xxxu.conf /etc/modprobe.d/


### Realtek RTL8192eu Automated uninstall/reset

Run from driver directory:

./uninstall_wifi.sh

## Install Flatpak
$ flatpak install brave bitwarden flatseal

## Install UFW and fix reboot bug
https://devtidbits.com/2019/07/31/ufw-service-not-loading-after-a-reboot/

$ sudo apt-get install ufw gufw
edit /lib/systemd/system/ufw.service
backup the ufw service file
$ sudo cp /lib/systemd/system/ufw.service /lib/systemd/system/ufw.service.bak
add as the final line of the [unit] description
    After=netfilter-persistent.service


## Install VNC
https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-vnc-on-debian-11

$ sudo apt install xfce4 xfce4-goodies

$ sudo apt install tightvncserver

$ sudo apt install dbus-x11

$ vncserver
Output
You will require a password to access your desktops.

Password:
Verify:

$ vncserver -kill :1

$ mv ~/.vnc/xstartup ~/.vnc/xstartup.bak

$ nano ~/.vnc/xstartup
add 
#!/bin/bash
xrdb $HOME/.Xresources
startxfce4 &

$ vncserver
output
New 'X' desktop is ben-10ay0003au:1

Starting applications specified in /home/XXX/.vnc/xstartup
Log file is /home/ben/.vnc/XXX-10ay0003au:1.log

$ sudo chmod +x ~/.vnc/xstartup

$ 
### Connect via ssh
$ ssh -L 5901:127.0.0.1:5901 -C -N -l XXXX your_server_ip
