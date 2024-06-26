# Debian 12 Bookwom Post install

## install bash completion
sudo apt-get install bash-completion


## Install flatpack
https://flatpak.org/setup/Debian
Error due to hardware clock not correct
Error Can't load uri https://dl.flathub.org/repo/flathub.flatpakrepo: While fetching https://dl.flathub.org/repo/flathub.flatpak repo: [60] SSL peer certificate or SSH remote key was not OK
see
[Debian bug report #1041049](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1041049)


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

#### Blacklist kernel drivers

$ echo "blacklist rtw88_8822bu" | sudo tee /etc/modprobe.d/rtw8822bu.conf

## Install Realtek RTL8192eu
https://github.com/clnhub/rtl8192eu-linux

$ $ sudo apt install linux-headers-generic build-essential dkms git

$ git clone https://github.com/clnhub/rtl8192eu-linux

$ cd rtl8192eu-linux

$ sudo ./install_wifi.sh




#### Realtek RTL8192eu Automated uninstall/reset

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
##### Connect via ssh
$ ssh -L 5901:127.0.0.1:5901 -C -N -l XXXX your_server_ip


## SSH user key creation
https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

$ ssh-keygen -t ed25519 -C "your_email@example.com"


### Encrypt HOME folder for user(s)
Debian has a page for this, however it lists using gocryptfs as an alternative.
https://web.archive.org/web/20231225202210/https://wiki.debian.org/TransparentEncryptionForHomeFolder

Try the gocryptfs method as per the following link.  
https://web.archive.org/web/20230418185508/https://leighmcculloch.com/posts/ubuntu-encrypt-home-directory-with-gocryptfs/


### Time and Date synchronisation
$ sudo aptitude install systemd-timesyncd

$ sudo joe /etc/systemd/timesyncd.conf

[Time]
NTP=0.au.pool.ntp.org  1.au.pool.ntp.org 2.au.pool.ntp.org 3.au.pool.ntp.org
FallbackNTP=0.debian.pool.ntp.org 1.debian.pool.ntp.org 2.debian.pool.ntp.org 3.debian.pool.ntp.org
RootDistanceMaxSec=5
PollIntervalMinSec=32
PollIntervalMaxSec=2048
ConnectionRetrySec=30
SaveIntervalSec=60

$ sudo timedatectl set-ntp true

$ timedatectl status
               Local time: Sat 202
           Universal time: Sat 2024-01-13 04:05:54 UTC
                 RTC time: Sat 2024-01-13 04:05:54
                Time zone: Australia/
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no



The user has already been added.

$ sudo apt install libpam-mount gocryptfs

### MPWR BH519A BLUETOOTH DONGLE install
Install instructions https://unix.stackexchange.com/questions/637599/bluetooth-dongle-drivers  

Driver  
https://web.archive.org/web/20230626001053/https://www.xmpow.com/pages/download  


archived version of the driver  
https://web.archive.org/web/20230626001053/https://cdn.shopify.com/s/files/1/0249/2891/1420/files/20201202_BH456A_driverforLinux-1_0929.7z?v=1664445632



Possible alternative driver  
https://web.archive.org/web/20211209231716/https://mpow.s3-us-west-1.amazonaws.com/mpow_BH519A_driver+for+Linux.7z
