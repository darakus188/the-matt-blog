+++
title = "Fedora 30 on a Low End Laptop"
date = 2019-07-07

[taxonomies]
categories = ["Technology"]
tags = ["fedora", "linux", "install", "setup"]
+++

This morning I decided to dust off an old laptop that has been sitting in my closet for a year or two and see what I could do to revive the old clunker. This has always been kind of a toy laptop to me and it was about as cheap as you can get in the United States. I got it on sale on Black Friday at OfficeMax a few years ago for $159. It's a Toshiba Satellite C55-B5299 that sports a 15 inch 1366x768 display. The internals are about what you would expect at such a low price: A dual-core Intel Celeron N2830 (2.16GHz/2.41GHz turboboost) processor, 2GB of RAM, and a 500GB 5400rpm laptop hard. My goal was to get this old slow computer to run decently well for watching videos and web browsing for my nieces. And in case you didn't notice the title, to accomplish this task I'm using my favorite flavor of Linux: Fedora.

<!-- more -->

![Image of a terminal with the output of neofetch command. It shows the software and hardware described in this article.](https://matthewbunt.org/laptopneofetch.png "Terminal running neofetch")

Before starting this I knew that the standard Fedora Workstation with the GNOME desktop would be too heavyweight for this aged hardware. However, I wanted to see just how bad it would run and so I installed Workstation anyway just to test. I'll tell now, I was not wrong. After using the Fedora Media Writer tool from my desktop to create a bootable flash drive with the F30 Workstation ISO on it, I decided that I would try to switch the laptop back into UEFI mode as when I bought it that wasn't working so well on Linux yet. After booting off of the USB drive I was happy to find out that Fedora happily installed itself and I didn't have to switch back into legacy bios mode. Success!

I selected all of the defaults and automatic partitioning and just let Anaconda do it's thing. It took about 30 minutes but afterwards I had a working F30 Workstation on my laptop. Unfortunately for this laptop, GNOME is really made for modern hardware and works best with 4GB or more of RAM. With only 2GB, my laptop was constantly using swap space on the hard drive which slowed things to a crawl after opening a program or two. To my surprise, the desktop animations were fairly smooth even on this machine. Since opening Firefox used up all of the remaining RAM and then some, using GNOME for this computer was clearly a no go. 

With a fond farewell to GNOME, I downloaded the LXDE spin of Fedora. This is the most minimal desktop that is offered by Fedora as a spin. There are of course many more desktop setups you can do that are smaller and leaner than LXDE, but I am lazy so I want an installer to do the hard work for me. Both the download and the install time were much shorter with LXDE, perhaps even half the time it took to install the full Fedora Workstation. This version of Fedora also installed without a hitch and I was soon booted into a decent looking desktop that uses less than 350MB of RAM while idle. Now, to get things setup on this new installation.

The first thing you do after installing any Linux distro is update. After invoking `dnf upgrade` there was a 700MB or so download of new packages and then the installation took a while with the slow processor and disk speeds. The whole transaction took perhaps 10 minutes.

After the upgrade completed I needed to install some packages to get things rolling. Sadly, since LXDE includes dnfdragora as a GUI package manager instead of GNOME Software, I had to install the RPMFusion repositories the old fashioned way. I downloaded the .rpm packages for both the free and non-free and I installed them using dnf. *Oh the horror!* I might have been able to install them with dnfdragora but I'm not really sure. If I don't have GNOME Software I tend to use dnf. To tell the truth, even if I do have GNOME Software I still use dnf quite often. For anyone unfamiliar with Fedora, RPMFusion is a project that hosts some repositories for Fedora users that contain packages that for mostly legal/license reasons RedHat can't distribute with Fedora directly. They contain things like steam and other proprietary software. What I want today though is ffmpeg-libs and its accompanying codecs. Installing ffmpeg-libs is almost certainly overkill for this situation, but I just like the ease of installing one package to get all video playback working. After also installing Firefox I went to youtube.com/html5 and it showed my browser supporting all of the features and formats. The only other packages I installed were levien-inconsolata-fonts, vim, gnome-screenshot and neofetch so I could edit text and capture the screenshot shown above.

At this point I could have stopped. Video was playing in Youtube and Netflix without any issue. However, there were still a couple of annoyances I needed to fix. First, I am a dark theme kind of guy so I changed the LXDE theme to adwaita-dark which fixed most of the UI but the bottom panel was still white. So I opened the panel settings and edited the color to match the rest of the theme. While I was modifying the panel I also added a Firefox quick launch icon as well as enabled the volume control and battery indicator plugins.

The last thing I configured was the first thing I noticed when I turned on the system. The touchpad had tap-to-click disabled. I searched through the LXDE settings but sadly there were only mouse settings to be had. I was forced to resort to creating an X11 configuration file:

`/etc/X11/xorg.conf.d/30-touchpad.conf`
```
Section "InputClass"   
  Identifier "touchpad"  
  Driver "libinput"  
  MatchIsTouchpad "on"  
  Option "Tapping" "on"  
EndSection
```
Thanks to user887114 over on [askubuntu.com](https://askubuntu.com/questions/1087328/lubuntu-18-10-how-to-activate-tap-to-click) for that!

All in all it was an enjoyable morning. It's something I've done dozens of times in the past but it seems like every time I either learn something new or notice improvements in Fedora. That really is one of the coolest things about using an open source operating system. If you are interested in such things, you can closely follow the advancement of the software and see it improve and change over time. It really is a fun experience and something I want to become more a part of. So thanks to all the Fedora developers and contributors for doing so much to move Linux into the future!
