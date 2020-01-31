+++
title = "Shutting down for the night"
date = 2020-01-31

[taxonomies]
categories = ["Linux"]
tags = ["ssh", "linux", "config"]
+++

When I get ready to go to sleep at night I do what I suspect many others do. I turn on Netflix. I watch Star Trek as I'm going to bed. I cycle through the original series and the Next Generation, to Deep Space Nine, Voyager and finally Enterprise. I watch it on my computer, but usually don't even make it through one episode before falling asleep. Because of this I set my computer to shutdown two hours after I get ready for bed. I have always done this by issuing the shutdown command with a timer on the command line. This has worked well enough but occasionally I will forget to issue the command and I will inevitably have to get out of bed, go back to my computer and issue the shutdown order. Being physically disabled this is quite a hassle for me. So I decided that I would use technology to solve the problem, I am a Linux geek after all.
<!-- more -->

Initially I downloaded an ssh app that just let me log in and gave me a terminal. It was a little clunky using a terminal from my phone so I decided that maybe I would take a slightly different route. I did a bit of research and found an app that let me setup a command and then push a button on my phone to execute the command via ssh. So I created a button that sends the shutdown command with a 2 hour timer to my computer. Excited to try my new solution, I pushed the button. And…the command returned code 1. That is Linux' way of telling me that something didn't work. A return code of zero means the command executed successfully, anything else is a failure code. I double checked to see if a shutdown was scheduled. Nope.

So I decided to try to get a better error message. I opened a terminal and connected to localhost via ssh and entered my normal shutdown command (shutdown -h +120) which resulted in the following error:
```
Failed to set wall message, ignoring: Interactive authentication required.
Failed to power off system via logind: Interactive authentication required.
Failed to open initctl fifo: Permission denied
Failed to talk to init daemon.
```
This was a big surprise to me because the exact command executed from a local terminal (not ssh) would work as expected. My user can normally use the shutdown command without sudo, but I decided to try it with sudo anyway because the error mentions authentication and permissions. Using sudo with the shutdown command via ssh worked. I entered my password and the shutdown was scheduled. However, my [phone app](https://play.google.com/store/apps/details?id=com.pd7l.sshbutton) had no way of prompting for the sudo password. I have no interest in enabling the root account so I decided to ask the opinions of other Linux geeks and see if anyone had suggestions. I got several quick responses on [Ask Fedora](https://ask.fedoraproject.org/t/non-interactive-shutdown-from-ssh/5135) and after a bit of back and forth I was glad to get the proper solution.

What I needed to do was configure sudo in a way that my user would be allowed to use shutdown without requiring a password. While I can run shutdown locally without privilege escalation, running it from ssh requires the extra privileges that sudo grants. The solution involves editing the sudo configuration file. Because this file is so important there is a tool called visudo that will open the sudo config file in vi and then after you do your edits it will validate the changes to make sure you didn't break anything. I added the following to my sudo config file (replace user with your username):
```
user ALL=(ALL) NOPASSWD: /sbin/shutdown
```
Now when I enter the command "sudo shutdown -h +120" my ssh app returns code 0 and also the time the shutdown will happen. It worked out pretty well and with the help of my fellow Fedora users, I have a solution to my problem.

Extra Credit:
At some point since I have been doing this shutdown procedure something changed in Fedora that caused my computer to use the internal pc speaker (the beep speaker) to 'warn' that a shutdown is imminent. When trying to drift off to sleep this is quite distracting. However, after a quick google, the solution seems quite simple but involves editing another config file. Create the file if it doesn't exist.

/etc/modprobe.d/blacklist.conf:
```
blacklist pcspkr
```

After a restart, sweet silence is all you'll hear from the beep speaker.
