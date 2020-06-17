# Kivy-kiosk
Creating Kivy kiosk 
Ubuntu server 18.04  Kivy kiosk

First we need to install a fresh Ubuntu server 18.04 . After the installation is complete, make sure to install any available updates by executing the following commands:

```
sudo apt-get update
sudo apt-get upgrade 
sudo reboot
```

To install Kivy enter the following:

```
sudo apt-get install python3-pip
sudo apt-get install cython3 python3-dev
sudo apt-get install libsdl2-dev libsdl2-ttf-dev libsdl2-image-dev libsdl2-mixer-dev
sudo python3.6 -m pip3 install git+https://github.com/kivy/kivy.git@master
```

To install a minimal X11 on Ubuntu Server Edition enter the following:
```
sudo apt-get install xorg
```
Next install a Window Manager:

```
sudo apt-get install openbox
```
The Openbox configuration file is located at “/etc/xdg/openbox/autostart”. We can open it for editing with the following command (replace user and pyton script):

```
sudo nano /etc/xdg/openbox/autostart
```

#Disable any form of screen saver / screen blanking / power management
xset s off
xset s noblank
xset -dpms

#Allow quitting the X server with CTRL-ATL-Backspace
setxkbmap -option terminate:ctrl_alt_bksp

#Start Kivy program
(python3 /home/[user]/[python script].py) &


The main part of the configuration is done. To start the GUI simply type “startx” in the console, the GUI can be closed by pressing ctr+alt+backspace.

Autostart Graphical user interface

We can not call a system a kiosk if we need to manually type “startx” after every reboot. So lets make sure we wont have to.

We can accomplish this by configuring the “.bash_profile” file for the auto login user. Assuming we are alredy logged on as the auto logon user, use the following commands to create or open the “.bash_profile” file for editing:

```
cd
sudo nano .bash_profile
```
now append the following line to the file:

[[ -z $DISPLAY && $XDG_VTNR -eq 1 ]] && startx
 
```
sudo nano /etc/default/grub
```

GRUB_CMDLINE_LINUX_DEFAULT="plymouth:debug splash vt.handoff=7 kaslr"
```
sudo apt-get install plymouth plymouth-themes
sudo update-alternatives --install /usr/share/plymouth/themes/default.plymouth default.plymouth /usr/share/plymouth/themes/spinfinity/spinfinity.plymouth 100

sudo update-alternatives --config default.plymouth
sudo update-initramfs -u

sudo cp /home/qms/ubuntu-logo.png /usr/share/plymouth/ubuntu-logo.png
```
