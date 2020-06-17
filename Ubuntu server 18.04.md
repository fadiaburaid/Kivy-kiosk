# Ubuntu server 18.04 Kivy kiosk with autostart custom splash screen

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

# Install GUI components, window manager and configure auto boot
If you tried to run your Kivy application in Ubuntu server you will get an error  **Unable to get a Window, abort** .
This is because it Ubuntu server doesn't come with GUI and window manager.

To install a minimal X11 on Ubuntu Server Edition enter the following:
```
sudo apt-get install xorg
```
Next install a Window Manager:

```
sudo apt-get install openbox
```
The Openbox configuration file is located at “/etc/xdg/openbox/autostart”. We can open it for editing with the following command :

```
sudo nano /etc/xdg/openbox/autostart
```
Then we append the following (replace user and pyton script)

```
# Disable any form of screen saver / screen blanking / power management
xset s off
xset s noblank
xset -dpms

# Allow quitting the X server with CTRL-ATL-Backspace
setxkbmap -option terminate:ctrl_alt_bksp

# Start Kivy program
(python3 /home/[user]/[python script].py) &
```

The main part of the configuration is done. To start the GUI simply type “startx” in the console, the GUI can be closed by pressing ctr+alt+backspace.

Autostart Graphical user interface

We can not call a system a kiosk if we need to manually type “startx” after every reboot. So lets make sure we wont have to.

We can accomplish this by configuring the “.bash_profile” file for the auto login user. Assuming we are alredy logged on as the auto logon user, use the following commands to create or open the “.bash_profile” file for editing:

```
cd
sudo nano .bash_profile
```
Then append the following line to the file:
```
[[ -z $DISPLAY && $XDG_VTNR -eq 1 ]] && startx
```
Now you can reboot to test whether your Kivy application will run automatically after booting by typing.
```
sudo reboot
```


# Set a custom splash screen with your choosen image

Install the plymouth and themes tool on Ubuntu
```
sudo apt-get install plymouth plymouth-themes
```
Change the theme to your preferance (Here I choose spinfinity)
```
sudo update-alternatives --install /usr/share/plymouth/themes/default.plymouth default.plymouth /usr/share/plymouth/themes/spinfinity/spinfinity.plymouth 100
```
Then run the below command.

```
sudo update-alternatives --config default.plymouth
sudo update-initramfs -u
```
To change the boot image replace it using the following command(replace your image path)

```
sudo cp [your image path] /usr/share/plymouth/ubuntu-logo.png
```
Edit the file /etc/default/grub to enable plymouth splash by typing:

```
sudo nano /etc/default/grub
```
Edit the  GRUB_LINUX_DEFAULT line to be:

```
GRUB_CMDLINE_LINUX_DEFAULT="plymouth:debug splash vt.handoff=7 kaslr"
```
To update grub to new settings
```
sudo update-grub
```
Reboot your system to see the results
```
sudo reboot
```
# Image of result
![alt text](https://github.com/fadiaburaid/Kivy-kiosk/blob/images/boot.gif?raw=true)
