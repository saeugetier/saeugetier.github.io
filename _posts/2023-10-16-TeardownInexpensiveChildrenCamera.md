---
title: "Teardown: Inexpensive Children Camera"
date: 2023-10-16
---

I recently purchased several inexpensive children's cameras on AliExpress. These cameras are available from various shops, priced between 4 to 15€. I ordered four samples from different sellers, noticing slight variations in build quality and firmware. These variations were evident through different button layouts and the presence of missing or malfunctioning features.

![Camera](/assets/2023-10-16/Camera1.jpg)

In the camera's menu, I found these version strings:

- MMI Version: YX_Q5_EN_7690_NV3023_B09-11-2023 22:59:12 LCD: 0x3025/0x3025
- AX3295B_22-08
- X100-9225-0308
- 3023A

Given their low price, I didn't have high expectations. These are, after all, toys, hopefully durable enough for children's use. Curious about their affordability, I disassembled them to explore their internals.

The camera with the longest version string had the most firmware issues. Attempting to record a video caused the firmware to crash. This particular camera differed in hardware, lacking a microphone and featuring a different main controller. 

![Camera](/assets/2023-10-16/Camera2.jpg)

Internally, there isn't much complexity. Basic structure of all cameras is very similar, but each sample can be differed in used components. Two cameras used a 300mAh Lipo Battery, while the other two had batteries without a marked capacity, and appeared slightly smaller. Surprisingly, I didn't notice a battery protection circuit for the smaller batteries. The 300mAh battery lasted about 30 to 40 minutes with display on. Perhaps a hardware modification to include a larger battery is feasible? Each camera had a different, cheap loudspeaker. Interestingly, one camera with a different main processor lacked a microphone. They all supported micro SD cards for photo storage. All four camera sensors look different.

![Camera](/assets/2023-10-16/Camera3.jpg)

I first disassembled the camera with the long version string. The main processor in one variant is a Spreadtrum SC6531F, a budget mobile phone processor with a GPRS modem. It appears to support Bluetooth and Wi-Fi, but lacks antennas for these functions.

Datasheet: [https://datasheetspdf.com/mobile/1455703/Spreadtrum/SC6531E/1](https://datasheetspdf.com/mobile/1455703/Spreadtrum/SC6531E/1)

While searching for the Spreadtrum chip I found that there is a port of Doom for that processor. There is a precompiled version for this kind of children camera. The children camera is listed with the name "YX Q5" as compatible device: [https://github.com/ilyakurdyukov/fpdoom](https://github.com/ilyakurdyukov/fpdoom). There is a hint, that pressing the shutter key on bootup will enter the bootloader. I could reproduce it by connecting it via USB while pressing the shutter button.

The datasheet indicates that the processor can handle up to 5MP sensors in JPEG mode using a CCIS camera interface. Quick search in the internet gave me no information about this interface. It can also support 0.3MP sensors without JPEG compression.

Considering the 24-pin camera connector, I initially suspected an OV2640 camera chip, a 2MP sensor also supporting JPEG mode. The OV2640 is a very cheap and commonly used sensor. However, based on the firmware version string, the more likely candidate is the OV7690, a 0.3MP VGA sensor. Both camera sensors interfaces have a lot in common.

[OV7690 Datasheet](http://aitendo3.sakura.ne.jp/aitendo_data/product_img/camera/OV7690/ov7690_full.pdf)
[OV2640 Datasheet](https://www.uctronics.com/download/OV2640_DS.pdf)

This variant of camera stores photos at 640x480 pixels, suggesting a VGA sensor. Despite having settings for up to 40MP, these don't seem to affect image quality.

The display might be a NV3023A display. There is a hint in the first version string. The last firmware version string could be a date code or a reference to the used display. The port of Doom also indicates, that a NV3023 display is used.

[NV3023A Product Page](https://www.panelook.com/TVT0130D-I-Hot-Stock-IPS-1-3-inch-128x128-SPI-interface-square-TFT-LCD-Screen-300nits-NV3023A-detail_155467.html)

Opening the other cameras reveals that the other three cameras feature a different hardware variant.

![Camera](/assets/2023-10-16/Camera4.jpg)

To access the main processor, I had to disassemble the PCB, but unfortunately, the chip's markings were removed. The main processor doesn't have an internal flash. Right beneath it is a SPI flash.

![Camera](/assets/2023-10-16/Camera5.jpg)

The second firmware version hints at the processor possibly being a Jinrixin Technology AX3295B.

[AX3295B Product Page](https://en.jrxchip.com/product/10.html)

There isn't much public information about this chip. It's also possible that an AX3291 is used. One possible hint is, that there is no Wi-Fi implemented and the description of the AX3291 explictly lists USB and memory card interfaces.

[AX3291 Product Page](https://en.jrxchip.com/product/9.html)

The camera's menu allows setting image resolutions up to 40MP. At this setting, images are stored at 4032x2880 pixels, around 12MP. However, given the image quality, it's more likely that a sensor similar to the OV7690 is used, with images being software-upscaled. However the live preview shows better flicker reduction and white balance. As visible below the captures have different image characteristics. The AX3295B is listed to be compatible with an OV9732 which interface seems to similar to the OV7690. One interesting experiment for future will be to interchance the camera sensors of all hardware variants.

When connected to a PC, this variant functions as a mass storage device and webcam, with the webcam's maximum resolution at 640x480 pixels, indicating a likely VGA sensor. I found no key combination for entering a bootloader mode which would allow to flash different firmware.

Both main hardware variants have very similar user interfaces implemented in firmware, using the same UI images. However, the button layout differs in the SC6531F variant. The firmware version suggests that newer hardware revisions might use the Spreadtrum chip, and perhaps future updates will fix video recording issues.

Under good lighting, the camera can capture clear images with low resolution. Indoors, however, the images are quite noisy. These cameras are indeed inexpensive toys, suitable for teaching children basic camera handling, hopefully leading them to handle more advanced cameras responsibly in the future.

Image shot by SC6531F variant:

![Photo](/assets/2023-10-16/DSC_0000289.jpg)

Images shot by AX3295 variant:

![Photo](/assets/2023-10-16/PHO00461.JPG)

![Photo](/assets/2023-10-16/PHO00473.JPG)

![Photo](/assets/2023-10-16/PHO00386.JPG)

Various overlays are available for decoration or framing.
