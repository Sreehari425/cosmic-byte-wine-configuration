---
 
---

# Cosmic Byte Device Configuration Guide

## Disclaimer

  **This guide is independent and not affiliated with or endorsed by Cosmic Byte. The Cosmic Byte software remains the property of Cosmic Byte, and users are responsible for downloading it from the official website. This guide simply assists users in configuring the software on Linux via Wine.**



## Table of Contents

1. Introduction

2. Dependencies

3. System Requirements

4. Installation Steps

5. Configuration Steps

6. Testing and Validation

7. Warnings and Security Risks

8. Tested Systems

9. Conclusion

10. GitHub Repository

## Introduction

This guide provides step-by-step instructions for configuring Cosmic Byte devices on Linux using Wine. It focuses on enabling device detection and functionality for applications that require proprietary drivers.

**<u>*Note : This only works with devices which uses human device interface(HID</u>)***

## Dependencies

- **Wine**: A compatibility layer to run Windows applications on Linux.

- **Winetricks**: A script to help manage Wine dependencies.

- **Microsoft Visual C++ Redistributable**: Required for certain applications.

- **Microsoft WebView2**: Needed for rendering web content.

- **.NET Framework**: Necessary for some applications.

- Note: install any other dependencies which your specfic app may require

## System Requirements

- **OS**: Linux

- **RAM**: Minimum 4 GB

- **Storage**: Minimum 1 GB free space

- System requirment may vary depending on the software 

## Installation Steps

#### **if you using any other distribution other than the one's below install the dependencies from your distributions package manager**

1. **Install Wine**
   
   1. Debian/Ubuntu
      
      1. `sudo apt install wine`
   
   2. Arch
      
      1. `sudo pacman -S wine`

2. **Install Winetricks**
   
   1. Debian/Ubuntu
      
      1. `sudo apt install winetricks`
   
   2. Arch
      
      1. `sudo pacman -S winetricks`

3. **Install Microsoft Visual C++ Redistributable**
   
   1. Go to your browser and go to  Visual C++ Redistributable Runtimes All-in-One and select the website www.techpowerup.com
   
   2. After Downloading it extract the archive
   
   3. it should contain a bat file which is named 'install_all.bat' along those name
   
   4. using wine excute the bat file `wine install_all.bat`
   
   5. Note: You could use any other Visual C++ Redistributable Runtimes All-in-One the above one is just an example

4. **Install Microsoft WebView2**
   
   1. `winetricks webview2`

5. **Install .NET Framework** (if required)
   
   1. `winetricks dotnet48`

6. **Download Cosmic Byte Software**
   
   1. Visit the official Cosmic Byte website and download the installer for your device.
   
   2. **Configure Cosmic Byte Application**
      
      1. Install your respective software that you downloaded previously
         
         1. `wine your_application.exe`
         
         2. replace **your_application.exe** with the installer files name.
         
         3. after installation it should be added as desktop entry
         
         4. or it usualy installed in ~/.wine/drive_c in this directory it might be in Program Files(x86) or Program Files
         
         5. After finding the path or runing it via desktop entry file follow the next step

7. Install any other depend which you dependencies keyboard/mouse software may need
   
   ## Configuration Steps

1. **Identify the Keyboard**
   
   1. List USB devices to ensure the Cosmic Byte keyboard is recognized by the system:
   
   2. Note down the **Vendor ID** and **Product ID** (vendor_id:product_id)
      
      1. `Bus 002 Device 003: ID 1234:5678 VendorName ProductName`
         
         Here, `1234` is the `vendor ID`, and `5678` is the `product ID`.
   
   3. To check weather you keyboard use Human Device Interface
      
      1.    Use `lsusb -v -s Bus_number:device_number`
      
      2. Look for `bInterfaceClass` if it says `Human Interface Device` you are good to continue

2. **Create Udev Rule for HID Permissions**
   
   1.   Create a udev rule to allow proper access to the HIDRAW device
      
      1. `sudo nano /etc/udev/rules.d/99-cosmic-byte.rules`
      
      2. Add the following content, replacing the Vendor ID and Product ID:
         
         1. `SUBSYSTEM=="hidraw", ATTR{idVendor}=="1234", ATTR{idProduct}=="5678", MODE="0666"`
      
      3. **Reload Udev Rules**
         
         1. `sudo udevadm control --reload-rules`
         
         2. `sudo udevadm trigger`
      
      4. **Test Keyboard in Wine**
         
         1. Launch the Cosmic Byte software using Wine
            
            1. `wine path/to/cosmicbyte.exe`
         
         2. Check if the software detects the keyboard and allows configuration



### Testing and Validation

- After configuring the Cosmic Byte software, test all features (lighting, macros, etc.) to ensure the keyboard is functioning correctly.

- the keyboard is not detected, confirm the udev rules are applied correctly using
  
  - `ls -l /dev/hidraw*`
    
    - Ensure the correct permissions (`rw-rw-rw-`) are set.

### Warnings and Security Risks

- **Permissions**: Setting `MODE="0666"` allows all users to read/write to the device. Consider using stricter permissions for security (`0660`), or limit access to a specific user group.

- **Running Windows Applications**: Wine does not isolate Windows programs as well as virtual machines, so be cautious of running untrusted software.

- **USB Device Access**: Ensure only necessary devices have access through udev rules to prevent potential security risks.

### Tested Systems

This guide has been tested on the following systems:

- **Operating System**: Arch Linux (6.10.10-zen1-1-zen Kernel)
- **Desktop Environment**: KDE Plasma 6.1.5
- **Wine Version**: 9.18
- **Hardware**: 
  - **CPU**: Intel Core i5-4440s
  - **GPU**: AMD RX 570
  - **RAM**: 12 GB
  - **Peripherals**: Cosmic Byte CBGK-15 (INSTANT USB Keyboard)

While this guide should work on other Linux distributions and desktop environments, compatibility may vary depending on hardware and software configurations. For best results, ensure your system is updated and configured similarly.

### Conclusion

- **Please note that while this guide provides a solution for many Cosmic Byte devices, compatibility with all models or other vendor keyboards is not guaranteed. If you experience issues, check for additional dependencies or specific configurations for your device.**

- Following these steps will allow you to run Cosmic Byte's software or other vendors software on Linux, enabling you to configure your keyboard (or other HID-based devices). Be mindful of security implications and system requirements. Feel free to contribute, report issues, or suggest improvements via the GitHub repository.

### GitHub Repository

[GitHub - Sreehari425/cosmic-byte-wine-configuration](https://github.com/Sreehari425/cosmic-byte-wine-configuration)

    
