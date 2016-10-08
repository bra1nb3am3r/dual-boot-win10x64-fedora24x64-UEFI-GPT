# Dual boot Windows 10 x64 with Fedora 24 x64 (UEFI &amp; GPT)

Names of some options are most probably different - I have written them down as I remembered.
It all worked for me just fine but it does not mean it will work the same way for anyone else.
I take no responsibility for any damages and loses.

The steps below are not detailed... yet.

## Preinstall
1. Create bootable USB using [Rufus](https://rufus.akeo.ie/downloads/).
2. Make sure to select `GPT for UEFI`.
2. Turn off `fast boot` (control panel) and `secure boot` (uefi settings)
3. Shrink partition using Windows `Disk Management`.
4. Windows Shift-Click `Restart`, Choose `Troubleshooting` then `UEFI USB Device`

## Install process
1. Boot from USB
3. Install Fedora to hdd
4. Choose `User partitioning`
5. Click `Create default partiotion/mounting points` and adjust it.
6. Reboot and remove usb.

### Suggested partitions:
```
    500 MB (boot)         ~  0.50 GB
  15360 MB (root)         ~ 15.00 GB
  21004 MB (home)         ~ 20.00 GB
   4096 MB (swap)         ~  4.00 GB (RAM)
-------------------------------------
  40960 MB (unallocated)  ~ 40.00 GB
  root + home = 36364 MB  ~ 35.50 GB
```

## After install
Most of the informations are  based on/taken from:
-  http://www.2daygeek.com/
-  http://www.tecmint.com
-  https://itsfoss.com/

Instead of using `sudo`, you can use `su` and after one time root password prompt do whatever you want at your own risk.

### Update and restart
```
sudo dnf update
sudo dnf upgrade
systemctl reboot
```

### Change hostname
```
hostnamectl
hostnamectl set-hostname “<hostname>”
```

### Assign higher privileges to user (optional, IMHO it is better not to do it)
`usermod -a -G wheel <username>``

### Install development tools and libraries
```
sudo dnf groupinstall 'Development Tools'
sudo dnf groupinstall 'C Development Tools and Libraries'
```

### Install and enable RPM Fusion repositories
`su -c 'dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm'`

### Install Java (OpenJDK)
`sudo dnf install icedtea-web java-openjdk java-1.8.0-openjdk-headless.x86_64`

### Java (Oracle JDK/JRE)
Choose verstion and install.
-  [Oracle JRE](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html)
-  [Oracle JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

### Install more wallpapers
`sudo dnf install f21-backgrounds-extras-gnome f21-backgrounds-extras-kde f21-backgrounds-extras-xfce f21-backgrounds-extras-mate f22-backgrounds-extras-gnome f22-backgrounds-extras-kde f22-backgrounds-extras-xfce f22-backgrounds-extras-mate f23-backgrounds-extras-gnome f23-backgrounds-extras-kde f23-backgrounds-extras-xfce f23-backgrounds-extras-mate f24-backgrounds-extras-gnome f24-backgrounds-extras-kde f24-backgrounds-extras-xfce f24-backgrounds-extras-mate`

### Install packages
Some of them are not essential and some might be already installed - remove the ones you do not plan to use.
`sudo dnf install tomahawk clementine vlc keepass guake nano vim vim-enhanced unzip gimp inkscape mc tomboy dpkg yumex-dnf gparted konqueror dolphin nautilus curl wget uget aria2 qbittorrent transmission deluge youtube-dl evolution corebird gnome-tweak-tool dconf-editor bleachbit tlp tlp-rdw redshift nautilus-dropbox wireshark aircrack-ng httpd php phpMyAdmin git nodejs npm nodejs-grunt nodejs-grunt-cli python ruby rubygems android-tools usbutils system-config-firewall p7zip p7zip-plugins unrar cabextract lzip brasero`

### Install media codecs
`sudo dnf install gstreamer-plugins-bad gstreamer-plugins-bad-free-extras gstreamer-plugins-bad-nonfree gstreamer-plugins-ugly gstreamer-ffmpeg gstreamer1-libav gstreamer1-plugins-bad-free-extras gstreamer1-plugins-bad-freeworld gstreamer1-plugins-base-tools gstreamer1-plugins-good-extras gstreamer1-plugins-ugly gstreamer1-plugins-bad-free gstreamer1-plugins-good gstreamer1-plugins-base gstreamer1 ffmpeg`

__Update and reboot.__

### Check if there are any original packages - choose RPM packages and install.
-  [Adobe Flash Player](https://get.adobe.com/pl/flashplayer/)
-  [Google Chrome](https://www.google.pl/chrome/browser/desktop/)
-  [Opera](http://www.opera.com/pl/computer)
-  [Vivaldi](https://vivaldi.com/download/)
-  [Atom](https://atom.io/)
-  [Sublime text](https://www.sublimetext.com/3)
-  [NetBeans IDE](https://netbeans.org/)
-  [Mega](https://mega.nz)
-  [Dropbox](http://dropbox.com/)
-  [Viber](http://www.viber.com/en/products/linux)
-  [Skype](https://www.skype.com/pl/download-skype/skype-for-computer/)
-  [Telegram](https://desktop.telegram.org/)
-  [WhatsApp](https://www.whatsapp.com/download/)
-  [Wickr](https://www.wickr.com/personal#medownload)
-  [Wire](https://wire.com/download/)
-  [Line](https://line.me/en/download)
-  [Android SDK](https://developer.android.com/studio/index.html)

### Install Viber
`sudo dnf install http://download.cdn.viber.com/desktop/Linux/viber.rpm`

### Install Skype Alpha for Linux
`sudo dnf -y install https://repo.skype.com/latest/skypeforlinux-64-alpha.rpm`

### Install Telegram (IMHO the one from Fedy is a better choice.)
```
sudo dnf copr enable rommon/telegram
sudo dnf copr enable iranzo/telegram-cli
sudo dnf install telegram-desktop telegram-cli
```

### Install Dropbox
```
sudo dnf install https://www.dropbox.com/download?dl=packages/fedora/nautilus-dropbox-2015.10.28-1.fedora.x86_64.rpm
cd ~ && wget -O - "https://www.dropbox.com/download?plat=lnx.x86_64" | tar xzf -
~/.dropbox-dist/dropboxd
```

### Install Fedy
`bash -c 'su -c "curl http://folkswithhats.org/fedy-installer -o fedy-installer && chmod +x fedy-installer && ./fedy-installer"'`

### Install PostInstallerF
```
su
rpm --import https://raw.githubusercontent.com/kuboosoft/postinstallerf/master/GPG/$(rpm -E %fedora)/RPM-GPG-KEY-postinstallerf
wget -P /etc/yum.repos.d/ https://raw.github.com/kuboosoft/postinstallerf/master/postinstallerf.repo
dnf clean all && dnf -y install postinstallerf
```

### Install rEFInd
```
su
wget http://netassist.dl.sourceforge.net/project/refind/0.10.3/refind-0.10.3-1.x86_64.rpm
rpm -Uvh refind-0.10.3-1.x86_64.rpm
```

### Install Android SDK
```
su
cd /opt
wget https://dl.google.com/android/android-sdk_r24.4.1-linux.tgz
sudo tar -zxvf android-sdk_r24.4.1-linux.tgz
cd /android-sdk-linux/tools
./android
```

### Install Wine
`sudo dnf install wine`

If standard installation of `wine` failed, try the one belowe (in my case both failed).

```
su
dnf install libX11-devel freetype-devel zlib-devel libxcb-devel
dnf config-manager --add-repo https://dl.winehq.org/wine-builds/fedora/24/winehq.repo
dnf makecache
dnf install winehq-devel
```

### Install antivirus

#### ESET NOD32 (failed in my case)
wget http://download.eset.com/download/unix/eav/eset_nod32av_64bit_pl.linux

If you are installing from a downloaded file, right-click the file, `Properties - Permissions`,
select the `Allow file to run as a program` and close the window. Double-click the file to start the installer.

#### ClamAV
`sudo dnf install clamav`

### Install different desktop environtments

#### KDE Desktop
`sudo dnf install @kde-desktop`

#### MATE Desktop
`sudo dnf install @mate-desktop`

#### CINNAMON Desktop
`sudo dnf install @cinnamon-desktop`

#### XFCE Desktop
`sudo dnf install @xfce-desktop`

#### LXDE Desktop
`sudo dnf install @lxde-desktop`

### Add [Gnome Extensions](https://extensions.gnome.org/)

### Add online accounts
`Show Applications (Menu) –> Settings–> Online Accounts.``

### Clean it up
```
sudo dnf autoremove
sudo dnf clean all
```

### Troubleshooting

#### System Settings does not work
`sudo dnf reinstall libwbclient`

#### Time mismatch
If while running Windows (after running Linux) the time changes under Windows,
execute commands below from terminal:
```
timedatectl
timedatectl set-local-rtc 1
```

### Some more...

#### Keepass translation (optional)
Download & extract
```
cd ~/Pobrane
su
cp ./Polish.lngx /usr/lib/keepass
```

#### Set Static IP address
Open and edit your network configuration file called enp0s3 or eth0 (or sth. similar)
under `/etc/sysconfig/network-scripts/` directory. Open this file with the editor of your choice with root privileges.
```
su
atom /etc/sysconfig/network-scripts/ifcfg-enp0s3
```
Sample Output:
```
HWADDR=08:00:27:33:01:2D
TYPE=Ethernet
BOOTPROTO=dhcp
DEFROUTE=yes
PEERDNS=yes
PEERROUTES=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
IPV6_FAILURE_FATAL=no
NAME=enp0s3
UUID=1930cdde-4ff4-4543-baef-036e25d021ef
ONBOOT=yes
```
Now make the changes as suggested below and save the file.
```
BOOTPROTO="static"
ONBOOT="yes"
IPADDR=192.168.0.200
NETMASK=255.255.255.0
GATEWAY=192.168.0.1
DNS1=202.88.131.90
DNS2=202.88.131.89
```
__Important:__ make sure to replace network configuration in the above file with your own network settings.  
After making above changes, make sure to restart the network service to take new changes into effect and verify the IP address and network settings with the help of following commands.
```
service network restart
ifconfig
```

#### Enable and configure ssh
```
systemctl enable sshd; systemctl start sshd
systemctl status  sshd
```
Disable Root login: edit `/etc/ssh/sshd_config` and set `PermitRootLogin` to `no`.  
Restart the SSH service: `systemctl restart sshd`.

