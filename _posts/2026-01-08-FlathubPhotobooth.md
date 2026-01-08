---
title: "Photobooth: Distribution moved to Flathub"
date: 2026-01-08
---

For a long time I packaged the Photobooth as a custom Yocto image. Yocto gave me the control I needed: a minimal system, a carefully built Qt, and a boot-into-application setup that works well on dedicated hardware. But maintaining an entire Linux distribution for a single application turned out to be a heavy burden.

Yocto forces you to think about the whole operating system. Kernel versions, device tree fragments, distro recipes, and cross-compiled libraries all require ongoing maintenance. When I started adding new features—most notably a YOLO v11 based image segmentation pipeline for background removal—build-time Python dependencies became a real pain. Some Python packages needed to build parts of the toolchain and were awkward to integrate into Yocto recipes. Rebuilding or updating a single Python dependency often meant long build times and fiddly recipe work.

Moving the distribution to Flathub (Flatpak) changed the workflow in one decisive way: I no longer have to manage the whole OS. With Flatpak/Flathub I package the application and its direct dependencies. Many libraries and runtimes are already available as prebuilt Flatpak runtimes or exported by the Flathub ecosystem. That means:

- I only worry about the app and its dependencies, not kernel or system packaging.
- Precompiled binaries for common libs shorten CI and keep iteration fast.
- Python dependencies can be handled in a user-space environment (bundled or provided by runtime), avoiding complex Yocto recipe work.
- Distribution becomes cross-distro: one Flatpak on Flathub runs on many desktop distributions.

An additional practical advantage was build speed and CI integration. The Yocto build for the full image took about four hours on my laptop. Building the application as a Flatpak is noticeably faster, and Flatpak integrates well with GitHub Actions. I can build a test Flatpak in pull requests and publish an installable artifact for quick testing on other machines. This lets me iterate features and verify platform-specific issues without rebuilding a complete OS image.

For the Photobooth project this made adopting the YOLO-based background removal much simpler. The heavy lifting for segmentation (C/C++ libraries, OpenCV, inference runtimes) can rely on prebuilt components or be shipped as part of the Flatpak bundle. Python modules required to build or run the new feature are treated like normal application dependencies instead of cross-compiling them into a whole system image.

Operationally the migration brought other niceties: faster release cycles, simpler CI, and fewer surprises when users run the app on different distributions. Sandboxing in Flatpak also helps keep the application environment predictable without having to freeze a full OS image.

Another practical reason for the switch was Qt's lifecycle. The Photobooth used Qt5 for many years; Yocto recipes and my custom images still relied on Qt5. With Qt5 moving out of mainstream support, staying on Yocto would have meant continuing to carry an increasingly outdated Qt stack. One big task during the transition to Flathub therefore was porting the entire application to Qt6.

Porting to Qt6 briefly broke the existing camera support based on gphoto2. Previously the camera integration used a Qt Multimedia plugin wrapping gphoto2, but the plugin approach used before isn't supported the same way in Qt6. To regain camera functionality I removed the plugin dependency and implemented the camera input directly in the application. Using `QVideoFrameInput` I feed frames from gphoto2 into Qt's pipeline as a custom video source. Re-implementing the features that the plugin provided took some effort, but the port forced a cleanup of the multimedia code and resulted in a simpler, more maintainable implementation.

One challenge I ran into was getting the Flatpak metadata right. Documentation for manifest fields and best practices is fragmented, and there was no simple, local way to fully validate that metadata. A full metadata check is implemented in the Flathub distribution repository, so the most complete validation only happens in that packaging repo's CI. That made iterating on the manifest awkward at first, until I started using PR-built Flatpak artifacts and the distribution CI to catch the remaining issues.

I still appreciate what Yocto gave me: a small, reliable appliance image with tight control over boot and hardware. For my development and wider distribution goals, however, Flathub is a much better fit—significantly less maintenance while giving users an easy install experience.

If you want to try the app, it's now available on Flathub: https://flathub.org/de/apps/io.github.saeugetier.photobooth

You can also check out the source on GitHub: https://github.com/saeugetier/photobooth




