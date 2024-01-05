---
title: "Retrospect: Photobooth Project"
date: 2024-01-05
---

A few years ago, I embarked on a Photobooth project. I describe it as 'started' because, while it is operational, it's not yet complete. The ideas and roadmap for the project are available in the [GitHub repository](https://github.com/saeugetier/photobooth). I plan to add features periodically.

The project began in January 2018. I constructed the hardware using components I had on hand: a Raspberry Pi 2, an old Canon DSLR, a 15-inch touchscreen, and I [designed a housing](https://github.com/saeugetier/photobooth_hardware) for the setup. To enable photo printing, I acquired a used Canon Selphy CP910 photo printer.

![Hardware](/assets/2024-01-05/Hardware.jpg)

However, hardware was only part of the equation. I needed software to run the system. My search for existing software compatible with a touchscreen and capable of running on a Raspberry Pi came up empty. Consequently, I decided to develop my own application.

As a C++ programmer specializing in hardware-oriented software (embedded firmware), I had some experience with Qt applications. At the time, QML was in its early stages but offered greater flexibility in design, prompting me to experiment with it. QML required an up-to-date version of Qt, but the Raspberry Pi's Debian-based operating system had outdated packages. The available version of Qt was too antiquated for effective QML usage. The only solutions were compiling Qt from source or opting for a different Linux distribution.

QML relies on an OpenGL backend for rendering, necessitating hardware acceleration. Unfortunately, only the Raspberry Pi's native operating system supported full graphics acceleration. Compiling Qt for the Raspberry Pi from source was a slow, labor-intensive process. Fortunately, another solution was [Yocto](https://www.yoctoproject.org/), a tool for building custom distributions through configurations. Yocto had ready-to-use recipes for building Qt, which I utilized to create [my own configuration](https://github.com/saeugetier/poky-photobooth), enabling the Photobooth application to run on the hardware.

Main features of the Photobooth software include:
- Full touchscreen UI control
- Self-timer for photo capture and storage.
- User-selectable collage layouts, defined via XML file.
- Filter selection for each photo in a collage.
- Printing of collages or individual photos through the photo printer.
- Display of snapshots on the menu screen.
- Transmission of configuration XML from a USB drive, with export options for photos and collages.

A few months ago, I recorded a demo of the application:

[![](https://markdown-videos-api.jorgenkh.no/youtube/fB2aQGPT-wg)](https://youtu.be/fB2aQGPT-wg)

Another video showcases an older version on actual hardware:

[![](https://markdown-videos-api.jorgenkh.no/youtube/FDwVrfnzoUo)](https://youtu.be/FDwVrfnzoUo)

Now, Raspberry Pi OS includes up-to-date versions of Qt, rendering the construction of a custom distribution unnecessary. However, this process was a valuable learning experience in embedded Linux, and it led to a fully customized system with a boot-into-application feature and a custom boot screen.

![Bootscreen](/assets/2024-01-05/Bootscreen.jpg)

Utilizing Qt/QML has provided flexibility and strong performance. I've even successfully ported the Photobooth software to Android, running it on an older LG G3 phone.

![Android](/assets/2024-01-05/Android.jpg)



