## SW resources
* Raspbian Stretch Lite, [torrent here](https://downloads.raspberrypi.org/raspbian_lite_latest.torrent)
  * June 2018 image
  * Release date: 2018-06-27
  * SHA-256:3271b244734286d99aeba8fa043b6634cad488d211583814a2018fc14fdca313

# Build steps

## Burn Raspbian image onto microSD card
* Format microSD card
  * For MS Windows machine:
  * App: SDFormatter [for MS Windows](https://www.sdcard.org/downloads/formatter_4/)
    * Format: FAT
    * Settings: Quick format, Size adjustment ON
    * (Formatting notes [here](https://www.raspberrypi.org/documentation/installation/noobs.md))
* Burn Raspbain image to microSD card
  * App: Etcher [for MS Windows](https://etcher.io/)
  * Burn image on to SD card
  
  ### Manual Pi Config
You will need to connect a keyboard to the RasPi, and use the screen to follow these steps.
* Login:
  * user: pi
  * pass: raspberry
* Rotate touchscreen
  * `sudo nano /boot/config.txt`
    * ... add new line
    * `lcd_rotate=2`
  * `sudo reboot`
* Config Pi
  * `sudo raspi-config`
    * <1> ...change user password for pi user (recommended)
    * <2> ... set network
      * GB Britain (UK)
    * <3> boot options
      * B2 Wait for Network at Boot -> No
    * <3> boot options
      * B1 Desktop / CLI
      * B2 Console Autologin
    * <5> Interfacing options - assuming you want to SSH into Pi for further build steps (recommended)
      * P2 SSH -> Yes
      * P6 Serial (if using UART comms to communicate with Arduino chip)
        * Enable login shell accessible over serial -> No
        * Enable Serial port hardware -> Yes
    * <7> Advanced Options
      * A3 Memory Split -> 128MB
    * Exit, reboot
