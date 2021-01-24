---
layout: single
title:  "Setting up Plex and SabNZBD on Raspberry Pi 4"
date:   2021-01-24 23:00:00 -0500
categories: DIY Raspberry-pi Network
comments: true
typora-root-url: ..
---

Hello everyone! I am very excited for today's project. Today, I will be setting up Plex and SABNZBD on a brand new Canakit Raspberry Pi 4 (4GB). 

This is a project I have wanted to do for a long time. The idea for this project stemmed from my regular media centre, aka my gaming pc, and my [previous blog](https://georgeciesinski.github.io/raspberry-pi/diy/network/PiHole-Setup/) about setting up Pi-Hole on a Raspberry Pi 3. This power hungry beast needs to be left on whenever I want to access my Plex library or download new content using SABNZBD. The obvious problems here are with power draw, reducing the lifespan of my gaming PC, and using up my computer resources if someone wants to stream my library while I am gaming. Frankly, it isn't that huge of a deal, but I yearned for a better way to host my library. 

# Raspberry Pi 4

In comes the [Raspberry Pi 4](https://www.raspberrypi.org/products/raspberry-pi-4-model-b/). It is the latest Raspberry Pi and is basically a palm sized computer/integrated circuit. This device is incredible. It comes with a capable processor and graphics, 4GB of ram in my case, the capability to connect two 4k displays simultaneously with it's two microhdmi ports, 2 x USB 2.0 and 2 x USB 3.0, and a wireless adapter to connect to wifi. 

There are so many different ways to use this board. For example, you can set up Raspbian on it for a lightweight and portable linux experience. If this is too heavy, you can set up diet pi and use it headless (no monitor, keyboard, or mouse) by connecting to the console via SSH. There are a multitude of open source projects as well. You can get Kodi and stream movies and TV from the internet. You can set up retropi and emulate many older generation consoles to play your favorite classics. I could go on for hours about the many ways you can use the Raspberry Pi 4. 

I ended up going with this board because I wanted the most powerful board they offered to future proof my server a bit. I am hoping this lasts me a few years so that I dont have to recreate it again for some time. After doing some research, I was very happy to discover that the Pi 4 can handle the Plex Server, and likely an NZB server at the same time, so i took the dive and purchased the kit from Canakit on Amazon. 

# Canakit

![canakit](/assets/images/Plex-1/canakit.jpeg)

I ended up choosing [this kit](https://www.amazon.ca/CanaKit-Raspberry-Starter-Kit-4GB/dp/B07WRMR2CX/ref=sr_1_1_sspa?dchild=1&keywords=raspberry+pi+4&qid=1611088889&sr=8-1-spons&psc=1&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUEzNzBSQjE1U05CSUtPJmVuY3J5cHRlZElkPUEwMTAwOTEyMTdLR0pNUzVaRkhHRCZlbmNyeXB0ZWRBZElkPUEwOTQzNTE2MVc4N0FMWUpFNjE0ViZ3aWRnZXROYW1lPXNwX2F0ZiZhY3Rpb249Y2xpY2tSZWRpcmVjdCZkb05vdExvZ0NsaWNrPXRydWU=) from Amazon for about $150. Apologies if this link breaks in the future. Basically, it is a standard Raspberry Pi kit which came with a number of different parts:

- Raspberry Pi 4 Model B
- Canakit case with a fan, and three heat sinks
- 32GB Samsung micro SD card
- USB stick with Noobs installed
- Micro HDMI to HDMI cable
- Power cable with a power switch adapter
- Instruction manual and information packets

![canakit-open](/assets/images/Plex-1/canakit-open.jpeg)

This is an excellent value for what you get in my opinion. The kit itself was very nicely packed, and easy to set up. The case came in three pieces that loosely snapped together for easy assembly and disassembly. 

![raspberry-case](/assets/images/Plex-1/raspberry-case.jpeg)

The bottom layer of the case had a fitted mold which allowed the pi 4 to be comfortably seated on it. 

![raspberry-assembly](/assets/images/Plex-1/raspberry-assembly.jpeg)

At this point, I decided to install the heat sinks, although this could have also been done first. 

![raspberry-heatsinks](/assets/images/Plex-1/raspberry-heatsinks.jpeg)

![raspberry-heatsinks-installed](/assets/images/Plex-1/raspberry-heatsinks-installed.jpeg)

The mid layer of the pi case easily snapped right onto the bottom layer. 

![raspberry-case-1](/assets/images/Plex-1/raspberry-case-1.jpeg)

The final step was to install the fan and top layer of the case. 

![raspberry-case-2](/assets/images/Plex-1/raspberry-case-2.jpeg)

This was also pretty straight forward. I took the time to orient the fan correctly so that air blows into the case, and not the other way. The fan came with a red and black wire, which plugged into pins 4 (red) and 6 (black) for a 5 volt connection. I later changed this to pins 1 (red) and 6 (black) as the 5 volt connection resulted in loud fan operation, and pin 1 allowed for 3.3 volts instead. Finally, I snapped the top layer of the case onto the pi. All that was left to do was to plug in the micro SD card, and then plug it into a TV along with a mouse and keyboard. 

# Operating System

The Raspberry Pi 4 came with Noobs preinstalled. Noobs is a simple operating system installer, and in our case, it came with Raspbian. Raspbian is a great default OS which comes with all sorts of utilities and apps. At this time, I am not sure whether I will keep it or go with something lighter such as diet pi. 

## Requirements

In order to help me decide the OS, I wanted to talk about my requirements. I want an operating system which can run two simultaneous servers: plex, and SabNZBD. Besides this, I need to connect to the pi to do a handful of things: 

- Install periodic updates
- Transfer files (my entire media library)
- Transfer individual .nzb files in the future for the SabNZBD server to pick up and start downloading

So I decided to test the default Raspbian installation. It runs far better than on the RPi 3 which struggled with browsing the internet. Raspbian looks like a traditional OS which includes a GUI, menus, and many other features. On one hand, it is pretty heavy considering I will not be using this often, but on the other hand, there may be times where I don't want to SSH into the console to do things, and might want to VNC instead. 

I decided that it is worth the risk to just use the included Raspbian installation and attempt the rest of the installations on that. But first, I decided to make the setup headless.

# Raspbian Headless

The main advantage of using a headless setup is that you don't need a dedicated keyboard, mouse, and monitor for the Raspberry Pi. Instead, it can be accessed using remote access (VNC), and using SSH. This is a must for my setup, as I intend to have the raspberry pi and it's hard drive chilling on a shelf and wired into ethernet. 

The below steps were taken after installing and updating Raspbian fully. 

## Static IP

First, we must set up the static IP. The first step is to decide on the static IP address. I have a range on my router where I can set up static IPs, so I had an idea of what I would use straight away. You also need the router IP and the DNS IP you will be using. If you're not sure about this, you can check out the instructions at [this blog.](https://pimylifeup.com/raspberry-pi-static-ip-address/)

Once you are ready, open your console, and type:

```
sudo nano /etc/dhcpcd.conf
```

This will open dhcpcd.conf in nano, which is a type of text editor that works through the console.

![dhcpcd](/assets/images/Plex-1/dhcpcd.png)

I am configuring the static IP for my ethernet connection, so I added the below lines to this file. Please note that `interface eth0` is referring to the ethernet, while `wlan0` refers to the wifi connection. Make sure you use the correct one.

```
interface eth0
static ip_address=<ip_address>
static routers=<router_ip>
static domain_name_server=<dns_ip>
```

Once finished, click `Ctrl+X` for exit, and then press `y` to confirm you want to overwrite this file.

## Enable SSH and VNC

I will be using both SSH to access the console, and VNC to access the graphic user interface for Raspbian. To enable this, I went to `Menu` > `Preferences` > `Raspberry Pi Configuration` which opened up the Raspberry Pi Configuration window to the `System` tab. 

![system](/assets/images/Plex-1/system.png)

While in this tab, I changed Boot to `To Desktop` and Auto Login to `Login as user 'pi'.` This is necessary if you will run it headless as the pi will login to the desktop as soon as it is rebooted. 

I then clicked the `Interfaces` tab which took me to the below screen. 

I changed `SSH` and `VNC` to Enable, then clicked OK. Once the Raspberry Pi detected the changes, it prompted me if I want to reboot, which I said Yes to. 

In order to use VNC on the Raspberry Pi, you need to ensure it is installed. Mine came installed by default. You can check this by clicking `Menu` > `Internet` and seeing if VNC Server appears in this menu. If it does not, you will have to install it using [these instructions.](https://www.raspberrypi.org/documentation/remote-access/vnc/)

## Connecting using VNC Viewer

Now that the VNC is setup on the Pi side, I installed VNC viewer on my mac. Once installed, I simply had to type the static IP I set into the search bar and pressed enter. It prompted me to login, and save the login details. Once this was done, I can connect to the Raspberry Pi with a double click. 

# Plex Server

Now that I am able to connect to my Raspberry Pi with VNC Viewer, it is time to install Plex. I used [this video tutorial](https://www.youtube.com/watch?v=zRj9mrwISZ8) which does an excellent job guiding you through the installation process with the entire process visually documented. It is far better than I can do in a text blog, but if you do prefer text, [this](https://pimylifeup.com/raspberry-pi-plex-server/) is the tutorial they followed in the video. Please read the next session before following the steps in the video.

## Properly mounting a USB Drive on Linux

One important thing I will note is that at the 3:00 point of the video I linked earlier, **Byte My Bits** discusses how Plex creates a new user (plex) and group (plex) which does not have access to some mounted drives by default. This is because Linux is very particular about permissions, and on Raspbian, the default user:group is pi:pi. As you might recall from the video, **Byte My Bits** says to open `/etc/default/plexmediaserver` in nano to edit the `PLEX_MEDIA_SERVER_USER` so that it is pi and not plex. 

According to **ChuckPa**, a Plex team member, this is not the recommended way to do this. The Raspberry Pi can only use USB drives such as flash drives, and external hard drives. These are mounted under the media folder and are given a different name each time they are mounted. It also assumes the drives mounted this way are temporary, and so it only grants the current user (the user pi on Raspbian) access to the drive. This is the reason why **Byte My Bits** asks you to change the Plex default user. 

Instead, ChuckPa has written a [very handy guide](https://forums.plex.tv/t/using-ext-ntfs-or-other-format-drives-internal-or-external-on-linux/198544) on how to mount your drive properly. As this guide is quite lengthy and detailed, I will summarize the steps I took below. Please note that if you have an NTFS drive like I do, you must install **ntfs-3g** if it isn't installed already.

```
sudo apt-get update
sudo apt-get install ntfs-3g
```

 When I tried to run the above commands, it turned out that my version of Raspbian already has ntfs-3g.

1. Open Terminal and type `df` which will show you a list of device names. An easy way to identify the USB storage disk is to type `df` once with the disk remove, and again with it plugged in. The device that shows up when you plug the disk in is the name of the drive. In my case, the extra device appeared as below. 

   ```
   /dev/sda2      3906885628 440792028 3466093600  12% /media/pi/Toshiba Ext
   ```

   Note the File System is `/dev/sda2` and the Mount Name is `/media/pi/Toshiba Ext`

2. We must create a directory to mount the File System to. To do this, we must enter root:

   ```
   sudo sh
   * Enter your password if prompted *
   #
   ```

   The hash symbol that appears indicates you are in super-user or root. You can exit this later by pressing **CTRL + D**. 

   While in root, we will create the new mount point. The below instruction creates the directory `/usb` and the subdirectory `/media` inside the `/usb` directory.

   ```
   # mkdir /usb /usb/media
   ```

   Next we assign the correct permissions (755 according to ChuckPa) to this directory:

   ```
   # chown -R pi:pi /usb
   # chmod -R 755 /usb
   ```

3. As I mentioned earlier, the device name changes every time you mount a USB Drive. Instead, we need the UUID  and the partition type which can be found with the below command. Note how we are entering the File System we found earlier (/dev/sda2). You need to use the correct one for your drive: 

   ```
   blkid /dev/sda2
   ```

   Result: 

   ```
   /dev/sda2: LABEL="TOSHIBA EXT" UUID="5C6A694A6A69224E" TYPE="ntfs" PTTYPE="atari" PARTLABEL="Basic data partition" PARTUUID="5559a221-b724-4f21-9f2d-231c47a8d238"
   
   ```

   Great, now we have the info `UUID="5C6A694A6A69224E" TYPE="ntfs"` which is exactly what we need.

4. The final step is to make a new entry in the file system table (/etc/fstab). As ChuckPa mentiones in his guide, the wrong information here can result in your drive not mounting correctly, and your OS possibly not booting, so please do this part carefully. 

   First we open /etc/fstab in the vi text editor:

   ```
   # vi /etc/fstab
   ```

   If you haven't used **vi** or **vim**, it starts in "command mode". This means you can't just start typing like a normal text editor. To start typing, press the `o` key which is the command used to enter "text input mode" and create a new line below your cursor. You can type normally at this point, so you can enter the below:

   ```
   UUID=(your UUID)  /usb/media  ntfs  defaults,auto,rw,nofail 0 1
   ```

   I wont go over the fstab file format as this can get complicated, but it is pretty easy to look it up online. I used the same information on this line as ChuckPa provided in his guide. 

   Now that you are done, you must save and exit the file. First, press `esc` to get out of "text input mode" and back into "command mode". Next, you can type `:wq` (including the : colon key) which stands for write & quit, and press enter. Now you just need to reboot, and assuming you entered the data correctly, the drive should now mount properly and with the correct permissions.

Well, that is pretty much it. Mounting drives properly in linux is obviously still a hassle, but it is worth it. The way this drive is mounted, Plex can utilize it perfectly and without issues. 

# Testing

Once this was setup, I went to the below URL to load the Plex web app:

```
http://[Local Plex Media Server IP Address]:32400/web
```

Of course, for the IP address I used the Static IP I set up earlier. I had about 1TB of Films and TV shows on the hard drive already, so I configured Plex to search those directories for the files. Plex went straight to work adding the files and downloading metadata for them. I did not check how long this took, but it wasn't very long. I was able to begin testing it in something like 10-15 minutes, although it may have still been downloading data in the background.

## Plex Web App

So the first thing I noticed is that the Plex Web App would often buffer high quality files roughly every 15 minutes. This did not happen with every file, but the highest quality files. Upon doing some research, I came to the conclusion that the web server causes the CPU to spike to 90-100% occasionally. I am not sure if this had anything to do with metadata being downloaded or if this was just the web server causing this spike. I did some research and ChuckPA from the Plex forums recommends not using web server if it goes slowly. 
My next steps with this is to research whether another Linux distro such as Diet-Pi which has no GUI desktop will be able to run the web app more efficiently. I have not looked into this much so I don't know how much performance this would actually save. 

## Plex TV App

My Samsung TV has a Plex App in the store. I downloaded this and logged into my account and was able to access my media library effortlessly. What I found most impressive is that there was no issues whatsoever streaming my Films and TV shows. It didn't seem to matter what quality they were either, they just worked. 
What is important to note here is that I had Direct Stream and Direct Play enabled. The raspberry pi 4 is not powerful enough to transcode files efficiently, but these options do not use transcode. If you are curious about the difference between these options, it is described [here](https://support.plex.tv/articles/200250387-streaming-media-direct-play-and-direct-stream/) on the Plex Support page. 

# Conclusion

At this time, I am very satisfied with the setup. My only regret is that I didn't research this more ahead of time to see whether diet-pi was a better OS choice. I went with the out of the box Raspbian which was included on the Noobs flash on the SD card that came with the kit. It seemed like the simplest solution at the time, especially since so many tutorials included Raspbian instructions. After completing this process, I would love to try the steps again but on diet-pi instead. 

My primary concern here is that some of the settings which could easily be accessed on Raspbian (auto login, etc) may be harder to find in Pi. It is also not possible to VNC, but from my experiencing connecting to the Raspberry Pi 4 using VNC, I don't know if I will ever use this in the future. This connection is slow, and there is no point of using the GUI at all. You can do most actions you will want to do with the Plex by using an SSH connection, and you wont have to watch the windows struggle to load. 

This project was an amazing learning experience that made me more familiar with Linux. I am very eager to pursue more projects with the Raspberry Pi including setting up more servers at home. I hope you enjoyed this blog post, and if you did, look out for more posts like this in the future!