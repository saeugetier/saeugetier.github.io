---
title: "Retrospect: Photobooth Project"
date: 2024-01-05
---

Some years ago I stared a Photobooth project. I say started, because it is usable, but still yet not completed. You can see the ideas and roadmap in the [github repository](https://github.com/saeugetier/photobooth). Features will be added from time to time.

I started the project in January 2018 by building the hardware with some stuff I had lying around. So I took a Raspberry Pi 2, an old Canon DSLR, a 15 inch touchscreen and [designed a housing](https://github.com/saeugetier/photobooth_hardware) for it. In order to being able to print out photos, I purchased a used Canon Selphy CP910 photo printer. 

![Hardware](/assets/2024-01-05/Hardware.jpg)

But building the hardware is not the end. I needed to run some software on it. I did some research, if there is any suitable software available yet. But I found nothing, which is usable via touchscreen and can run on a Raspberry Pi. So I decided to write my own application.

I'm a C++ programmer, mostly doing hardware oriented software (embedded firmware). But I had some experience in writing Qt applications. QML was just at the beginnings, but much more flexible in design, so I decided to give QML a try. Using features in QML required a up to date version of Qt. Back in time the Raspberry Pi operating system was based on Debian which had outdated packets. The shipped version of Qt was too old for reasonable use of QML. The only way to get an up to date Qt version, was compiling it from source or using a different Linux distribution.

QML uses a OpenGL backend for rendering, so hardware acceleration is a must. Unfortunately only the Raspberry Pi operating system was supporting full graphics acceleration. Compiling Qt for the Rasperry Pi from source is slow and requires a lot of manual work. Luckely an other way to compile Qt from source and have graphics acceleration is using [Yocto](https://www.yoctoproject.org/). Yocto can be used for building an own distribution by configurations. There are preconfigured receipes for building Qt available and ready to use. So I built my [own configuration](https://github.com/saeugetier/poky-photobooth) in order to execute the photobooth application on the Photobooth hardware. 

Main features of the Photobooth software:
- Full touchscreen control of UI.
- Capture and store of photos via selftimer
- User selectable collage layout. Available layouts are defined by XML file. 
- User can select a filter for each photo in collage.
- Collage or photo will be printed via photo printer.
- Snapshots are shown on menu screen.
- Configuration XML can be transmitted from USB drive. And photos and collages can be exported to USB drive.

Some months ago recorded a demo of the application:

[![](https://markdown-videos-api.jorgenkh.no/youtube/fB2aQGPT-wg)](https://youtu.be/fB2aQGPT-wg)

Another video is showing an older version on real hardware:

[![](https://markdown-videos-api.jorgenkh.no/youtube/FDwVrfnzoUo)](https://youtu.be/FDwVrfnzoUo)

Nowadays the Raspberry Pi OS ships with up to date versions of Qt. So going the more complicated way by building an own distribution isn't necessary anymore. But I learned a lot about embedded linux. And I have a fully customized experience with boot into application and custom boot screen.

![Bootscreen](/assets/2024-01-05/Bootscreen.jpg)

Using Qt/QML allows some flexibility and has a good performance. I even tried to port the photobooth software to Android. The application was running on a pretty old LG G3 phone.

![Android](/assets/2024-01-05/Android.jpg)



