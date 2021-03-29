# kstars_installation_instructions
Instructions on how to get kstars working on a raspberry pi 4

I could not find easy to follow instructions online so I am creating this to make it easier for others in the future.
Let me tell you from the beginning that a raspberry pi 4 was not able to handle running autoguiding and kstars at the same time.
While using the final working product with my DSLR camera and eq6-r pro mount the software crashed every ~10 minutes due to the low power of the raspberry pi.  However as a POC it worked very well.

## End goal of below steps
- A working kstars setup on a raspberry pi
- Ability to connect to raspberry pi through VNC without wifi (raspberry pi can create its own wifi network)
- Raspberry Pi can run the kstars/ekos software and move the mount/telescope without a hand-controller
- Raspberry pi can run PHD2 guiding software to maintain tracking accuracy and polar align / drift align

## Equipment required
- raspberry pi 4
- power cable + USB plug

## Equipment suggested
- cables to connect equipment after setup
- internet...
- astro-equipment
  - dslr/cmos camera
  - tripod + mount
  - guidescope + guide camera
  - battery for mount + camera
  - cables to connect equipment to battery + raspberry pi

## Pre-installation instructions
- setup the raspberry pi with "Raspberry Pi Imager"
  - Do not use a headless setup
- update raspberry pi
  - `sudo apt-get upgrade -yq && sudo apt-get update -yq && sudo apt-get upgrade -yq`
- reboot raspberry pi
  - `sudo reboot`
- setup vnc and ssh with raspi-config
  - `sudo raspi-config`
  - set vnc to on
  - set ssh to on
  - set boot to gui
  - set timezone to your localle

## Installation instructions
- download astroberry
  - `wget -O - https://www.astroberry.io/repo/key | sudo apt-key add -`
- update again
  - `sudo apt update`
- install indi
  - `sudo apt install indi-full gsc`
  - `sudo apt install indi-full kstars-bleeding`
- by default, python is not installed, install with below command
  - `sudo apt install python3 pip`
- now finish indiweb install
  - `sudo pip install indiweb`
  - `nano indiwebmanager.service` <-- paste in text from https://github.com/knro/indiwebmanager/blob/master/indiwebmanager.service
  - `sudo cp indiwebmanager.service /etc/systemd/system/`
  - `sudo chmod 644 /etc/systemd/system/indiwebmanager.service`
- enable the indiwebmanager service daemon so it is active on reboots
  - `sudo systemctl daemon-reload`
  - `sudo systemctl enable indiwebmanager.service`
- reboot & check status of indiwebmanager
  - `sudo reboot`
  - `sudo systemctl status indiwebmanager.service`
- Forgot what the below did but it seemed to work for me
  - `sudo apt-get install build-essential cmake git libeigen3-dev libcfitsio-dev zlib1g-dev libindi-dev extra-cmake-modules libkf5plotting-dev libqt5svg5-dev libkf5iconthemes-dev libkf5xmlgui-dev kio-dev kinit-dev libkf5newstuff-dev kdoctools-dev libkf5notifications-dev libqt5websockets5-dev qtdeclarative5-dev libkf5crash-dev gettext`
  - `sudo apt-get install gpsd`
  - `sudo systemctl enable gpsd`
  - `sudo systemctl restart gpsd`

*** At this point you should be able to reboot and load KSTARS/EKOS by loading going into the menu in your desktop and hitting 'run' then typing 'kstars' and clicking the application.

*** I reset my raspberry pi to use for other projects so i can't remember if the above steps were what it took to get the network coming from the raspberry pi or some other guide. But i remember that being easy to find online so if it is not already working you can find it elsewhere.  Feel free to link it to me and i'll update these instructions.

*** If wifi doesn't activate on the pi automatically check available connections on the raspberry pi.  It is possible the pi is putting out a wifi network but you need to connect to it on the PI before the pi automatically connects to it.  There should be a default password that includes the name of your pi...
