---
layout: single
title:  "Setting up Pi-Hole on Raspberry Pi 3"
date:   2020-12-27 23:00:00 -0500
categories: Raspberry-pi DIY Network
comments: true
typora-root-url: ..
---

I have been trying to find a use for my Raspberry Pi 3 for years. As a tech enthusiast, I have tons of spare phones, old computers, boxes of computer parts, and even an unused Raspberry Pi 3 lying around. 

![PiTop](/assets/images/PiHole/PiTop.jpeg)

I really like the case I got for this Pi as well, as it looks pretty sweet from the profile: 

![PiSide](/assets/images/PiHole/PiSide.jpeg)

In the past, I have tried to set up the Pi as a Plex Media Server, but the Pi 3 turned out to be very bad at encoding, resulting in choppy and laggy playback. I tried using it as a Retro Pie, but with Nes and Snes emulation now available on console, and plenty of options available for gaming, I found that I did not use it a lot. Then I came across the [Pi-hole](https://pi-hole.net). 

# What is Pi-hole

Pi-hole is a DNS level ad and malware blocker. Once it is set up and connected to your network, you can configure either the router or each device manually to use the Pi-hole as their DNS server. The Pi-hole uses a block list with known ad and malware URLs and blocks any requests to these URLs. Upon receiving such a request, the Pi-hole returns an empty value instead of the Ad the URL would usually return. The beauty of this method is that it can provide ad blocking outside of an internet browser. Ads in mobile games, or on Smart TVs can actually be blocked on a network level. Finally, Pi-hole provides a beautiful GUI which can be accessed over the network, allowing you to update block lists, and see which URLs have been blocked by Pi-hole.

# Perfect Utility for Raspberry Pi 3

This utility seemed perfect for the Raspberry Pi 3. You can run Pi-hole on a minimalist diet pi setup. You can also connect to the pi and configure it over the network so no mouse and keyboard is needed. The only things required are:

- Raspberry Pi 3
- A case to contain the Pi
- Micro-SD card
- Power cable for the Pi
- Ethernet cable to wire it to the router

Once it is setup, it can run quietly or accessed in order to check out logs. 

# Setup

Setting up Pi-hole was made very easy thanks to a [video](https://www.youtube.com/watch?v=4X6KYN1cQ1Y) uploaded by Jameson Malpezzi. He goes through everything I have done in this post in his video, so I recommend you watch it if you are setting up a Pi-hole on your own network. It is far more informative than this post, and contains all of the below info, and more.

## Installing OS on Raspberry Pi

First, I downloaded [Diet Pi](https://dietpi.com), which is a lightweight debian OS. As we will be primarily accessing the Pi using command line over SSH, it is not necessary to have any heavier OS with a GUI. 

![DietPi](/assets/images/PiHole/1-DietPi.png)

Once this was downloaded, I unzipped the folder. 

Next, I downloaded [Balena Etcher](https://www.balena.io/etcher/), which is a simple to use flash utility which lets you flash an image onto a disk. 

![Etcher](/assets/images/PiHole/2-BalenaEtcher.png)Once I installed Etcher on my Mac, I simply flashed the Micro SD card with the Diet Pi image, and the first step was complete. 

![FlashComplete](/assets/images/PiHole/3-FlashComplete.png)

Following installation, I plugged the Raspberry Pi 3 into power, and connected it to one of my routers with an ethernet cord. My Deco app notified me that DietPi has joined the network within minutes. 

## Connecting to DietPi via SSH for the first time

As I am running the DietPi headless (no screen), I needed to know the IP. Luckily, my Deco app found the new device and presented me with this information straight away. Otherwise, if you do not have a network app like this, you can use a tool like `nmap` to find the IP. This is better described in [this link](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md). Besides the IP, you also need to know the host name. The default hostname for the DietPi is `root`.

Once you have the host name (`root`) and IP, you can put them together into the below command:

```
ssh root@ip
```

The terminal will then prompt you to enter the password. The default pass for the DietPi is `dietpi`. Upon entering the correct password, you are greeted by a license you have to agree to in order to use the DietPi. 

After agreeing to the license terms, DietPi goes through an initial setup process including checking for and installing updates. This process takes a few minutes, but eventually shows you a page asking if you would like to opt in for a survey. 

![DietPiSurvey](/assets/images/PiHole/4-DietPiSurvey.png)

I skipped this in order to save time. 

This took me to yet another page asking if I would like to change the `default global software password` for dietpi. It was not recommended to be left as `dietpi` so I went ahead and updated it to something more secure. The next screen asked me if I want to change the existing unix user passwords. This was especially confusing to me as I do not know what is the difference between the global software password and the unix password. Nevertheless, I made both of these secure and moved on to the next step.

The next page asked if I want to disable Serial console as it is currently enabled. I followed the advice in the youtube tutorial linked earlier and left it as is. 

After I exited the last prompt, DietPi presented me with a menu with a number of different options. 

![DietPiMenu](/assets/images/PiHole/5-DietPiMenu.png)

I selected Install, which presented a message advising that no software has been found for installation and if we want to proceed with installing a pure DietPi image. Again, I opted to follow the advice of the tutorial and pressed ok. This went through another set of installs and updates which took around another minute, but finally resulted in a console screen with information, and some beautiful ascii art. 

## Installing PiHole

Once DietPi was installed and configured, I moved on to PiHole.

I went to the [PiHole Github Page](https://github.com/pi-hole/pi-hole), which included a very handy one-step automated install:

```
curl -sSL https://install.pi-hole.net | bash
```

I entered this into the DietPi console, and entered the above command. 

![PiHole](/assets/images/PiHole/7-PiHoleSetupScreen.png)

The automated install showed a visual check list which was automatically checked off, and eventually started spitting out logs into the console. 

![PiHoleChecklist](/assets/images/PiHole/8-PiHoleChecklist.png)

Finally, it presented me with the below message: 

![Transform](/assets/images/PiHole/9-NetworkWideAdblocker.png)

I clicked ok, and ok again on the next page requesting donations.

 The next page informed me that Pi-Hole is a server so it needs a static IP address. 

![StaticIP](/assets/images/PiHole/10-StaticIP.png)

We will be setting this later. Pi-Hole asked me to select an upstream DNS provider. I selected Cloudflare for this step. Next, Pi-Hole informed me that Pi-Hole uses third party lists to block adds and suggested a few to start. I clicked Ok. The next page asked me to select which protocols to use for the pihole. After looking into this, I selected IPV4 and IPV6 because both protocols are used to provide ads. 

The next page asked me to configure static IP. At this stage, it was necessary to go into my router settings and set a DHCP range which was outside the range of whatever I use as my static IP. This was pretty straight forward, and the exact method differs depending on which router you are using. I then went back to the Pi-Hole and set a custom IP of my choosing. I also made sure that the gateway it is using is correct.

Next, Pi-Hole asked if i wanted to install the web admin interface, which I said yes to as this is what we will use to configure the Pi-Hole in the future. I also clicked yes to the web server and the log queries prompts. For privacy mode, I selected option 2 (hide domains and clients) as I share this network with my room mates and wanted to respect everyone's privacy. 

The Pi-Hole then proceeded to scroll through more logs and check lists as it ran through the configuration based on the specified settings. Finally, it showed the resulting Pi-Hole IPs, and the web admin pass, and advised us to reboot the pi in order for the changes to take effect.

It is impostant to note that the Pi-Hole IP and the DietPi IP is still different, so in order for this to work correctly you must update the DietPi IP as well. To do this, you must enter the below into the console:

```
dietpi-config
```

This opens up a config menu. I scrolled down to `Network Options: Adapters` and then selected `Ethernet`. Here, I changed the default `DHCP` mode to `Static`. This presented additional options, including which static IP and gateway to use. I updated both to match the Pi-Hole settings. Once I did this, I told DietPi to save the settings and restart networking by selecting that prompt.

I had to quit the SSH connection as this was still attempting to connect to the old DHCP IP, and instead, I had to SSH back in with the new Static IP. Once connected, I double checked and confirmed that the DietPi was running correctly, and that the Pi-Hole admin page is reachable. 

The final step was to go to the DHCP server settings on my router and redirect all traffic to use the Pi-Hole as the DNS. Again, this is router specific so you have to look up instructions for how to do this on your specific router.

# Blocklists

Earlier, I mentioned how Pi-Hole uses third party lists to block ads. It is possible to add a few more to the basic ones you get, although you risk getting false positives as you are relying on someone else's list. 

To do this, you have to: 

1. Log into Web Admin Portal
2. Press `Settings`
3. Press `Ad Lists`, then `group management pages`
4. Enter the address and click Add

I went with the suggestions in the tutorial video linked earlier and added the recommended lists: 

- https://hosts.oisd.nl
- Various lists from https://firebog.net

# Testing

Finally, after setting up the Pi-Hole and setting all devices network wide to use it as a DNS, it was time to start testing. 

Upon opening the first test page, the first thing I noticed is how beautiful the web had become. The previously used for ads space in the pages turned into white space instead and improved the overall look of the page. 

Some news sites such as www.businessinsider.com complain and ask you to turn off adblock. Safari reported a nice long list of trackers from this site, so I was happy not being able to access it. Other sites like www.thewatchcartoonsonline.tv look amazing with all of the ads removed. This site in particular was riddled with banner ads and ads alongside the page content as well. It was terrible to have to scroll past everything constantly. 

# Conclusion 

Over all, I am very happy with the results and am looking forward to the next few weeks of testing this. I will no doubt have to add items to the white list as my room mates will inevitably report broken sites.

I highly recommend for anybody who knows their way around Raspberry Pi and other projects like this to set up this ad blocking solution to be used alongside a traditional ad blocker. 

I also hope that you liked this blog post. I love doing these DIY projects and learning new things. This project allowed me to learn about the built in SSH capability of the Mac Terminal, and how to better work with debian based operating systems. I learn a lot with every project, even the ones that I ultimately fail. 

With the start of the new year, you can expect more blog posts as I am ramping up my programming and updating my pages. See ya next time!



