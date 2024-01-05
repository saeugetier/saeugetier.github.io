---
title: "Teardown - Cheap Children Camera"
date: 2023-10-16
---

I recently ordered cheap children cameras on aliexpress. You can shops on aliexpress selling them from 4 to 15€. I ordered 4 samples of them from different shops. All have slightly different build quality. All come with different firmware, which can be noticed by different button layout or missing/malfunction features.

![Camera](/assets/2023-10-16/Camera1.jpg)

I found this version strings in the menu of the camera:

- MMI Version: YX_Q5_EN_7690_NV3023_B09-11-2023 22:59:12 LCD: 0x3025/0x3025
- AX3295B_22-08
- X100-9225-0308
- 3023A

But as this cameras are so cheap, I'm not expecting so much. It is a toy, which is hopefully not easy to destroy. But how are these cameras so cheap to produce? I disassembled them for looking in order to see what is inside. 

The one with the longest version string is the one with the most broken features. Trying to record a video will crash the camera firmware. This camera has different hardware than the others. For some reasons no microphone was soldered in and there is a different main controller.

![Camera](/assets/2023-10-16/Camera2.jpg)

There is not very much inside. All samples look a bit different inside, but all have mostly the same components. A Lipo Battery with 300mAh is used in two cameras. Two cameras have batteries with no capacity printed onto the battery. The unlabeled batteries seem to be a bit smaller in size. I couldn't see a battery protection circuit at the battery. 300 mAh will last about 30 to 40 minutes in use. Maybe this battery can be replaced by a bigger one as possible hardware mod? There is a very cheap loudspeaker. Every sample using a different loudspeakers. As mentioned before, there was one camera without a microphone. A micro sd card can be inserted for storing photos.

![Camera](/assets/2023-10-16/Camera3.jpg)

The main processor reveals to be a Spreadtrum SC6531F, which is a budget mobile phone processor with a GPRS modem inside. As far as I could see, the processor implements Bluetooth and Wifi as well. But there is no antenna for GPRS/BT/Wifi modems.

Datasheet: [https://datasheetspdf.com/mobile/1455703/Spreadtrum/SC6531E/1](https://datasheetspdf.com/mobile/1455703/Spreadtrum/SC6531E/1)

While searching for the Spreadtrum chip I found that there is a port of Doom for that processor. There is a precompiled version for this kind of children camera. The children camera is listed with the name "YX Q5" as compatible device: https://github.com/ilyakurdyukov/fpdoom

Reading the datasheet says that the processor can access maximum 5M pixel sensors in JPEG mode. The interface is an CCIS camera interface, which I never heard before. 0.3MP sensors can be used without JPEG compression.
Looking at the signal names and by the fact that the connector is a 24 pin camera connected, my first guess was, that the camera chip might be a OV2640. This camera chip is a 2M pixel camera sensor, which supports capturing in JPEG mode. The OV2640 is a very cheap and commonly used sensor listed for about 2€ on aliexpress. But after looking at the firmware version name, it is more likely that the OV7690 is used, which is a 0.3MP VGA camera sensor with a similar interface.

OV7690 Datasheet: [http://aitendo3.sakura.ne.jp/aitendo_data/product_img/camera/OV7690/ov7690_full.pdf](http://aitendo3.sakura.ne.jp/aitendo_data/product_img/camera/OV7690/ov7690_full.pdf)

OV2640 Datasheet: [https://www.uctronics.com/download/OV2640_DS.pdf](https://www.uctronics.com/download/OV2640_DS.pdf)

Photos are stored with 640x480 pixels, so it is very likely, that only a VGA camera sensor is used. The camera has quality settings up to 40M in the menu. But this settings seems to have no influence onto the image quality. 

The 3 other cameras have a different hardware variant.

![Camera](/assets/2023-10-16/Camera4.jpg)

The PCB has to be disassebled in order to see the main processor. Unfortunately the chip marking was removed. 

![Camera](/assets/2023-10-16/Camera5.jpg)

The second firmware version string spoilers a detail about the used processor (maybe). The main processor might be a Jinrixin Technology AX3295B. 

[https://en.jrxchip.com/product/10.html](https://en.jrxchip.com/product/10.html)

There is not maby public information about that chip. According to the available information it is also likely that the AX3291 is used:

[https://en.jrxchip.com/product/9.html](https://en.jrxchip.com/product/9.html)

The menu allows to set image resolutions up to 40M pixel, too. With that setting images are stored with 4032 by 2880 pixels on the sd card, which comes into the range of 12M pixels. According to the image quality it is more likely, that a similar sensor to the OV7690 is used and the images are "upscaled" by software. Looking at the live preview image, the sensor has less issues with flickering light adjustment and white balancing.

Pluggin this hardware variant into PC, the camera will show up as mass storage device and webcam. Maximum resolution of the webcam is 640x480 pixels. So it is very likely, that the camera sensor is has only VGA resolution.

The look and feel of the firmware of both hardware versions is very identical. Same images for the UI are used. The button layout of the SC6531F variant is different. The version string of firmware lets presume, that the Spreadtrum is used for newer hardware variants. Maybe the video recording function will be fixed in later firmware versions for the Spreadtrum chip. 

At good weather conditions the camera can capture images, where objects can be clearly identified. Inside of buildings the images are very noisy. The camera is really only a cheap toy, which can teach children how to handle a camera and hopefully at sometime they are ready to use real cameras without damaging them after first use.

Image shot by SC6531F variant:

![Camera](/assets/2023-10-16/DSC_0000289.jpg)

Images shot by AX3295 variant:

![Camera](/assets/2023-10-16/PHO00386.JPG)
There are some overlays which can be used as decoration or frame.

![Camera](/assets/2023-10-16/PHO00461.JPG)

![Camera](/assets/2023-10-16/PHO00473.JPG)
