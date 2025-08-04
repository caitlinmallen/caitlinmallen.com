---
layout: post
title: Emulating Horse Games on Windows XP Walkthrough
author: "Caitlin Allen"
date: 2025-08-03 16:19:55 +0300
description: A walkthrough for folks looking to emulate the “Let’s Ride” games on Windows XP
image: LetsRide_TitlePhoto.jpeg
tags: [Project, Gaming, Horses, Equestrian, Lets Ride]
---

First time posting something in four years!
About a year ago, I was trying to remember this specific horse game I played growing up. It was the ‘Let’s Ride Champion Seasons’ game, and not a whole lot seemed to exist on the Internet about it. I eventually found a YouTube channel that had posted some of the game and said they were emulating it and found the files on Internet Archive!
I posted an Instagram story once I set up my own Let’s Ride game in a Windows XP VM. Pretty quickly, I got a few requests for how I did this. I know a lot of people who had asked don’t work in tech or are not familiar with how to use VirtualBox. This guide is made to be accessible for all those who want to play their childhood horse game, whether or not they’ve ever even touched a VM.

![The Let’s Ride title screen]( /assets/img/LetsRide_Title.png){: .center-image }

## How Does This Work?
To give a TL;DR, you’ll be running another computer on your physical computer. This is done using a virtual machine, or VM for short. If you want to read a little bit more about what a VM is, VMware has a page with an overview.

Rather than using a physical CD-ROM, we’ll be using an Optical Disc Image, or ISO. This contains everything that would be on a physical disc, but it is now just a file on your computer. Lenovo has a brief overview about ISOs for those wanting to learn more.

## What You’ll Need

* Oracle VirtualBox - [download here](https://www.virtualbox.org/) and choose the package for your operating system
* Windows XP ISO – [download here](https://archive.org/details/XPProSP3ActivatedIE8WMP11)
* Let’s Ride ISO – In the bottom right corner where it says ‘ISO Image Files’, [download Disc 2](https://archive.org/details/Lets_Ride_Champions_Collection_ValuSoft_2003/Let%27s%20Ride%20Champions%20Collection%20%28ValuSoft%29%282003%29%2810280-2%29%28Disc%202%29.jpg) or use this [direct download of the file](https://archive.org/download/Lets_Ride_Champions_Collection_ValuSoft_2003/Let%27s%20Ride%20Champions%20Collection%20%28ValuSoft%29%282003%29%2810280-2%29%28Disc%202%29.iso)

## Setting up your VM

Once you’ve installed VirtualBox, we’ll be able to get started with setting up Windows XP.

At the top of the window, you’ll see a few options such as ‘New’ and ‘Add.’ Click on the ‘New’ blue sun(?) to open up the set up wizard.

![Not exactly sure what to call this shape, hence the (?)]( /assets/img/LetsRide_NewVM.png){: .center-image }

You’ll need to name your VM, choose where it’ll live on your computer, and upload the ISO file. Choose your name, use the default folder or change it to a preferred place, and then upload the ISO file from your computer. I personally put it on my desktop in this instance, but I typically make a dedicated folder (on my desktop, but that’s a me thing) where the ISO can live.

![My configurations]( /assets/img/LetsRide_SetUp1.png){: .center-image }

Set a username and password to use on your VM (you’ll be logged in automatically to an Admin account so this doesn’t exactly matter). You can enter a product key (they’ll be floating around online, but you can use XP without it), and a hostname. The hostname is a name for your VM.

![For reasons, I’ve censored the product key]( /assets/img/LetsRide_SetUp2.png){: .center-image }

You will have to now allocate resources to the VM so it actually can run. Since this is Windows XP, it does not need a ton of memory. I chose about 3GB since the VM became unstable when I started to bump it higher. One processor is also fine.
![My configurations]( /assets/img/LetsRide_SetUp3.png){: .center-image }

Next up is allocating actual space for the VM. I chose 30GB, but this is pretty overkill considering the ISO file is only 580mb.

![My configurations]( /assets/img/LetsRide_SetUp4.png){: .center-image }

The set-up wizard will now display a summary of your configurations.

![Summary of the configs on my end]( /assets/img/LetsRide_SetUp1.png){: .center-image }

Revisit that top menu bar and choose the ‘Start’ green arrow. Your VM will power up and pop open a new window. You’ll see this blue screen while it starts to boot up the new operating system.

![]( /assets/img/LetsRide_SetUp7.png){: .center-image }

The next Window will say it should take 40ish minutes to set up XP. It shouldn’t take more than a few minutes (if it does, stop the VM and restart.)
![My configurations]( /assets/img/LetsRide_SetUp8.png){: .center-image }

Once the operating system is ready to go, you’ll see this iconic screen and XP letting you know its applying your personal settings!

![Absolute icon]( /assets/img/LetsRide_SetUp10.png){: .center-image }

Before you fret about the screen size, we’ll load the game file into the computer. In the top menu, click Device > Choose/Create a Disk Image

![]( /assets/img/LetsRide_SetUp15.png){: .center-image }

Navigate to where your game file is and remember to choose Disc 2, since this is the game file with the Farnam Three Day eventing game. Once selected, click ‘Open’.

![]( /assets/img/LetsRide_SetUp16.png){: .center-image }

Now that you’re set, lets go over how to make your display a bit bigger.
![Don’t worry, it won’t be this small forever]( /assets/img/LetsRide_SetUp12.png){: .center-image }

In the top menu bar, go to View > Scaled Mode
![]( /assets/img/LetsRide_SetUp1.png){: .center-image }

You should get a pop up letting you know about the host key. This will be the Right Control key. I have two monitors, so I just bring my cursor over to my other monitor and click. But for those who only have one monitor, you’ll want to remember the host key.

![My configurations]( /assets/img/LetsRide_SetUp13.png){: .center-image }

In the start menu, click on My Computer

![]( /assets/img/LetsRide_SetUp17.png){: .center-image }

You should now see that there is a drive called Jumper (D:) in the Devices with Removable Storage section! Double click and we’ll install the game!

![]( /assets/img/LetsRide_SetUp18.png){: .center-image }

![]( /assets/img/LetsRide_SetUp19.png){: .center-image }

Click install now and go through the install wizard. You can leave the default settings and just click ‘Next’.

![Use Complete]( /assets/img/LetsRide_SetUp21.png){: .center-image }

Click ‘Yes’ when you get the DirectX 8 pop-up.

![My configurations]( /assets/img/LetsRide_SetUp22.png){: .center-image }

The time has come, click ‘Play Now’!

![Let’s Ride!]( /assets/img/LetsRide_SetUp23.png){: .center-image }

I was so freaking excited to finally get this set up and I’m so happy to share the knowledge with other horsey folks!

![]( /assets/img/LetsRide_Play1.gif){: .center-image }

My only qualm with the game is how I don’t exactly understand the dressage scoring (why am I losing points for walking when that is the gait I need to be doing?) and how you have to take what seems like you need to jump 4 strides out than you normally would (I’ve jumped maybe 7 times in my life, so don’t take my word for it).
You can also use this guide for other games from your childhood. As long as you can find the ISO file (which should be found with a few Google searches and a Reddit thread or two), you can play whatever games you want.
