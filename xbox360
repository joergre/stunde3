#!/bin/bash
dd if=/dev/zero of=/dev/sda bs=512 count=1
sfdisk /dev/sda << EOF
,124,S
,,L
EOF
mkfs.ext3 /dev/sda2
mkswap /dev/sda1
sync; sync; sync
swapon /dev/sda1
mkdir /mnt/debian
mount /dev/sda2 /mnt/debian
cd /mnt/debian
mkdir /mnt/debian/work
cd /mnt/debian/work
wget --no-check-certificate http://codin.ro/lynxbox/depend/debootstrap_0.3.3.2etch1_all.deb
ar -xf debootstrap_0.3.3.2etch1_all.deb
cd /mnt/debian
zcat < /mnt/debian/work/data.tar.gz | tar xv
export DEBOOTSTRAP_DIR=/mnt/debian/usr/lib/debootstrap
export PATH=$PATH:/mnt/debian/usr/sbin
debootstrap --arch powerpc etch /mnt/debian http://archive.debian.org/debian/
echo Xenon > /mnt/debian/etc/hostname
cat > /mnt/debian/etc/fstab << EOF
/dev/sda2     /          ext3     defaults   0   0
/dev/sda1     none    swap    sw           0   0
proc            /proc    proc    defaults  0   0
EOF
cat > /mnt/debian/etc/network/interfaces << EOF
iface lo inet loopback
auto lo
auto eth0
iface eth0 inet dhcp
EOF
cat > /mnt/debian/etc/apt/sources.list << EOF
deb http://archive.debian.org/debian/  etch main contrib non-free
EOF
cp /mnt/debian/root/.bashrc /mnt/debian/root/.bashrc.orginal
cat >> /mnt/debian/root/.bashrc << EOF
passwd
apt-get update
apt-get install ntp wget -y --force-yes
apt-get install x-window-system -y --force-yes
aptitude install gnome -y
apt-get install build-essential firefox gftp khexedit console-tools -y --force-yes
echo "AVAHI_DAEMON_START=0" > /etc/default/avahi-daemon
/etc/init.d/networking restart
cd /usr/lib/xorg/modules/drivers/
rm -r -f *
wget http://codin.ro/lynxbox/depend/xenosfb_drv.so
cd /etc/X11/
rm -r -f xorg.conf
wget http://codin.ro/lynxbox/depend/xorg.conf
mkdir /lib/modules/2.6.21.1
touch /lib/modules/2.6.21.1/modules.dep
echo "" > /etc/gdm/gdm.conf-custom
sed -i '/security/ a\AllowRoot=true' /etc/gdm/gdm.conf
sed -i 's/#LEDS=+num/LEDS=+num/' /etc/console-tools/config
update-rc.d -f hwclock.sh remove
update-rc.d -f festival remove
update-rc.d -f portmap remove
update-rc.d -f cupsys remove
update-rc.d -f spamassassin remove
update-rc.d -f alsa-utils remove
rm /root/.bashrc
mv /root/.bashrc.orginal /root/.bashrc
/etc/init.d/gdm start
EOF
echo "Base System Install Complete! Probably."
echo "You may now shutdown the xbox360."
echo "Then continue the install by booting the Xell-Bootloader-sda2 FROM A CD !!!."
