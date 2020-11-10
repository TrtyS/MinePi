# MinePi
Run Minecraft Server on your Raspberry Pi 4!
**For good preformance use ORIGINAL power supply and good cooling**, i have problems with my power brick and got throttle messages because undervoltage.
I recommend use RPi 4 at least 2GB model, not tested at RPi 3!

## WIP
I am woring on a automated script, come back after month :D

## How to do it
There is written quicky what I have done to host my Minecraft server, but still recommending to read the links to understand it *(with copying you wont learn anything) use ´man´*.
There are few steps missing *(creating separate user, raid, optimalization and service)*, I will feature them in the script (and also you can figure out yourself ;D).

**0)** Update + Resize - make sure all packages are current and you use you whole SD Card
´´´
sudo apt update
sudo apt upgrade
sudo raspi-config
´´´
In Advanced Options, click expand filesystem and then reboot with ´sudo reboot´.

**1)** Overclock & 64bit RPi - put this on end of the *config.txt*:
´´´
# overclock
over_voltage=6
arm_freq=2147
gpu_freq=750

# 64bit kernel
arm_64bit=1
´´´

**2)** Installing Software - we need git and java:
I figured out that openjdk 8 *(java 8)* **not work** on 64bit kernel, and openjdk 11 *(java 11)*, **wont generate files when spigot loaded**. For this reason,
we will use openjdk 10 *(java 10)*(we will be installing only jre package, because we need only Java Runtime Enviroment ;D).

´´´
sudo apt install git
sudo apt install openjdk-10-jre
´´´

**3)** Building Spigot - its minecraft server, but better
Download BuildTools from Spigot and compile them (you can specifiy versoin, more on website).
´´´
wget https://hub.spigotmc.org/jenkins/job/BuildTools/lastSuccessfulBuild/artifact/target/BuildTools.jar
java -jar BuildTools.jar --rev latest
´´´
Then you can remove BuildTools and run your server. You have to accept eula (edit eula.txt and rewrite false to true):
´´´
rm BuildTools.jar
java -jar server_version.jar

nano eula.txt
´´´
Now you can start your minecraft server and allocate with more RAM (I allocated 3GB on my 4GB model):
´´´
java -jar -Xms3G -Xmx3G server_version.jar nogui
´´´
Now your Minecraft server sucessfuly running on Raspberry Pi and if you have it in your network, you can acess it on ´raspberrypi.local´ on port *25565*.

# Links
There Are Articles When you can read, what you need. Try to figure it out :D

### [FOR INFO] Minecraft Server Reqirements
https://minecraft.gamepedia.com/Server/Requirements

### [OPTIONAL BUT RECOMMENDED] Overclocking And 64bit system
https://www.seeedstudio.com/blog/2020/02/12/how-to-safely-overclock-your-raspberry-pi-4-to-2-147ghz/
https://www.raspberrypi.org/documentation/configuration/config-txt/boot.md

### [OPTIONAL] Raid
https://l.messenger.com/l.php?u=https%3A%2F%2Fwww.ricmedia.com%2Fbuild-raspberry-pi3-raid-nas-server%2F&h=AT0sx7PDkhtOVrDlewXIzNqXORESOhoCgeHT-UCs7MkvM_RBgq2vfEpfKsZNQj-_qrFvVPcBScA8z_r0mhUNeHyBVLC3NZ_VdTsV7hZj9qjDNHy1uoHBFbRwTyWC8RNGq0tkEQ

### [OPTIONAL BUT RECOMMENDED] Create user
https://linuxize.com/post/how-to-create-users-in-linux-using-the-useradd-command/

### Spigot & Optimalization
**openjdk 8 not work at 64bit os, best alternative is openjdk 10 *(problems with openjdk 11 - spigot files not create)***
https://www.spigotmc.org/wiki/buildtools/
https://www.spigotmc.org/threads/guide-server-optimization%E2%9A%A1.283181/

### Creating Service & FIFO to communication with background process
**server will not start for some reason if the fifo is empty *(you have to fill for example with word start `echo "start" > fifofile`)***
https://www.raspberrypi.org/documentation/linux/usage/systemd.md
https://www.howtoforge.com/linux-mkfifo-command/
