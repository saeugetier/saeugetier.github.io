---
title: "Teardown - Cheap Children Camera"
date: 2023-10-16
---

#Teardown of a Cheap Children Camera"

I recently ordered cheap children cameras on aliexpress. You can shops on aliexpress selling them from 4 to 15€. I ordered 4 samples of them from different shops. All have slightly different build quality. All come with different firmware, which can be noticed by different button layout or missing/malfunction features.

![Camera](/assets/2023-10-16/Camera1.jpg)

I found this version strings in the menu of the camera:

- AX3295B_22-08
- MMI Version: YX_Q5_EN_7690_NV3023_B09-11-2023 22:59:12 LCD: 0x3025/0x3025
- X100-9225-0308
- 3023A

The one with the longest version string is the one with the most broken features. Tryng to record a video will crash the camera. Maybe this is due to missing microphone. All other samples have microphones soldered in. 

But as this cameras are so cheap, I'm not expecting so much. It is a toy, which is hopefully not easy to destroy. But how are these cameras so cheap to produce? I disassembled them for looking in order to see what is inside. 

![Camera](/assets/2023-10-16/Camera2.jpg)
![Camera](/assets/2023-10-16/Camera4.jpg)

There is not very much inside. All samples look a bit different inside, but all have mostly the same components. A Lipo Battery with 300mAh seems to be the standard, which last about 30 to 40 minutes. Maybe this battery can be replaced by a bigger one? There is a very cheap loudspeaker. Every sample using a different loudspeakers. As mentioned before, there was one camera without a microphone. A micro sd card can be inserted for storing photos.

![Camera](/assets/2023-10-16/Camera3.jpg)

The main processor reveals to be a Spreadtrum SC6531F, which is a budget mobile phone processor with a GPRS modem inside. As far as I could see, the processor implements Bluetooth and Wifi as well. But there is no antenna for GPRS/BT/Wifi modems.

Datasheet: https://datasheetspdf.com/mobile/1455703/Spreadtrum/SC6531E/1

Reading the datasheet says that the processor can access maximum 5M pixel sensors in JPEG mode. The interface is an CCIS camera interface, which I never heard before. But looking at the signal names and by the fact that the connector is a 24 pin camera connected, I assume, that the camera chip might be a OV2640.

Datasheet: https://www.uctronics.com/download/OV2640_DS.pdf

The camera chip is a 2M pixel camera sensor, which supports capturing in JPEG mode. The OV2640 is a very cheap and commonly used sensor listed for about 2€ on aliexpress.

The menu allows to set image resolutions up to 40M pixel. With that setting images are stored with 4032 by 2880 pixels on the sd card, which comes into the range of 12M pixels. According to the image quality it is more likely, that the OV2640 sensor is used and the images are "upscaled" by software. 

At good weather conditions the camera can capture images, where objects can be clearly identified. Inside of buildings the images are very noisy. The camera is really only a cheap toy, which can teach children how to handle a camera and hopefully at sometime they are ready to use real cameras without damaging them after first use.


https://github.com/ilyakurdyukov/fpdoom

At the end some example photos captured by the camera:

![Camera](/assets/2023-10-16/PHO00386.JPG)
There are some overlays which can be used as decoration or frame.

![Camera](/assets/2023-10-16/PHO00461.JPG)

![Camera](/assets/2023-10-16/PHO00473.JPG)





