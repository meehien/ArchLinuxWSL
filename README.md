# WSL Distro Launcher Reference Implementation
## Introduction
This is an C++ implementation for ArchLinux for Windows Subsystem for Linux (WSL) based on the Microsoft's reference implementation (https://github.com/Microsoft/WSL-DistroLauncher).

## Contents
This repository contains instructions on how to create an ArchLinux image for x64 e.g. `archlinux.tar.gz` and the VC++ code to generate the Universal Windows Platform (UWP) app for ArchLinuxWSL.

## Installing
1. Install a copy of Visual Studio 2019 Community from https://visualstudio.microsoft.com/downloads/. You will need the `Desktop development with C++` and `Universal Windows Platform development` components. Default configurations should be fine.

2. Generate a test certificate:
    1. In Visual Studio, open `VC++\DistroLauncher.sln`
    1. Select the `ArchLinux (Universal Windows)` project
    1. Select `ArchLinux.appxmanifest`
    1. Select the Packaging tab
    1. Select "Choose Certificate"
    1. Click the Configure Certificate drop down and select Create test certificate.
    1. Save the project and the solution.

3. Build the archlinux.tar.gz
    Roughly follow the ArchLinux install instructions from https://wiki.archlinux.org/index.php/installation_guide. Basic steps for creating the image on a linux distribution (preferably Archlinux):
    1. Install `arch-install-scripts` using pacman (on Archlinux) or by running `git clone https://git.archlinux.org/arch-install-scripts.git`
        1. If not on Archlinux compile pacstrap: `cd arch-install-scripts && make pacstrap`
    1. Create an install directory e.g. 'mkdir -p ~/archinstall'
    1. pacstrap the 'base' package, no need for the linux kernel in WSL: `pacstrap ~/archinstall base`
        1. Make required modifications to the files in `~/archinstall` either directly or using chroot; install additional packages with pacstrap e.g. `pacstrap ~/archinstall sudo git`; generate locale; etc.
    1. Create the image: `cd ~/archinstall && tar -zcpf ../archlinux.tar.gz --warning=no-file-ignored .`
    1. Copy `archlinux.tar.gz` from the Linux machine to the Windows machine where you are building the appx in the git directory, next to `VC++`.

4. Run the `VC++\build.bat` script.
5. The appx will be generated in `VC++\x64\Release\ArchLinux\ArchLinux_1.0.0.0_x64.appx` and will be signed with the self signed certificate from 'VC++\ArchLinux\ArchLinux_TemporaryKey.pfx'. This certificate needs to be installed as a Local Machine trusted root authority in order to be allowed to install the app.
