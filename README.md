# **IOT2050 Setting Up & Working with Example Image V1.5.x**

This documentation describes how to set up the SIMATIC IOT2050 with a SD-Card image `IOT2050 Example Image` provided through the Siemens Industry Online Support, how to remotely and locally access the IOT2050 and how to perform other basic tasks on the device.

![Setting-Up](/docs/graphics/1-iot2050-setting-up.png)

___

- [**IOT2050 Setting Up \& Working with Example Image V1.5.x**](#iot2050-setting-up--working-with-example-image-v15x)
  - [**Hardware structure**](#hardware-structure)
  - [**Required Hardware**](#required-hardware)
  - [**Required Software**](#required-software)
  - [**Operating**](#operating)
    - [**Installing the SD-Card Example Image**](#installing-the-sd-card-example-image)
    - [**Power Supply of the SIMATIC IOT2050**](#power-supply-of-the-simatic-iot2050)
    - [**Local access**](#local-access)
    - [**Remote ethernet access with SSH**](#remote-ethernet-access-with-ssh)
    - [**Remote serial access with UART connection**](#remote-serial-access-with-uart-connection)
    - [**User administration**](#user-administration)
    - [**Install software packages on the device**](#install-software-packages-on-the-device)
    - [**Change boot order in Example Image**](#change-boot-order-in-example-image)
    - [**Change boot order in in u-boot shell via UART connection**](#change-boot-order-in-in-u-boot-shell-via-uart-connection)
    - [**Skip eMMc as boot device**](#skip-emmc-as-boot-device)
    - [**Status LED's using Example Image**](#status-leds-using-example-image)
    - [**Basics of IOT2050-Setup Tool**](#basics-of-iot2050-setup-tool)
    - [**EIO controller firmware update**](#eio-controller-firmware-update)
    - [**IOT2050SM WebUI**](#iot2050sm-webui)
    - [**Restore IOT2050 Example Image**](#restore-iot2050-example-image)
  - [**Appendix**](#appendix)
    - [**Service and support**](#service-and-support)
    - [**Links \& Literature**](#links--literature)
  - [**Contribution and Contribution License Agreement**](#contribution-and-contribution-license-agreement)
  - [**Licence and Legal Information**](#licence-and-legal-information)

___

The IOT2050 Basic (6ES7 647-0BA00-0YA2) was used as the basis for the processes described here. However, the steps are generally valid for all IOT2050 models, but there may be slight deviations

|Model|MLFB|Description|
|-|-|-|
|IOT2050 Basic|6ES7 647-0BA00-0YA2|Basic IOT2050|
|IOT2050 Advanced|6ES7 647-0BA00-1YA2|including eMMc|
|IOT2050 M.2|6ES7647-0BB00-1YA2|including eMMc and interface for S7-1200 SM|
|IOT2050 SM|6ES7 647-0BA00-1AA2|including eMMc and M.2 interface on motherboard modules|

___

## **Hardware structure**

This chapter describes the hardware structure for IOT2050 Basic / Advanced / M.2 (right) and IOT2050 SM (left)

![IOT2050-Hardware-Structure](/docs/graphics/1-iot2050-hardware-structure.png)

|No.|Description||No.|Description|
|-|-|-|-|-|
|1|SD & SIM card slots||8|S7-1200 interface (IOT2050SM)|
|2|RESET button for the CPU||9|DisplayPort 1.1 A (IOT2050 Basic/Adv./M.2)|
|3|Ethernet interfaces 100/1000 Mbps||10|COM interface (RS232/422/485)|
|4|LED display||11|Top housing plastic (IOT2050 Basic/Adv./M.2)|
|5|Top housing||12|Top housing heat sink (IOT2050SM)|
|6|USB Type A||13|Power supply connector|
|7|USER button (programmable)||||

___

## **Required Hardware**

This chapter contains the hardware required for the setting up.

|**Hardware**|**Description**|
|-|-|
|**Micro-SD Card**|The SIMATIC IOT2050 operates with a Debian-based Linux operating system, which requires the use of a Micro-SD Card. The requirement for using SIMATIC IOT2050 with Debian based Linux Operating System is a Micro-SD Card with storage capacity from 8GB up to 32GB.|
|**Engineering Station**|To get remote access to the SIMATIC IOT2050 and to prepare the SD card an Engineering station with SD card slot and ethernet port is required. In this example a PC with Windows 11 is used.|
|**Ethernet cable**|For an Ethernet Connection between the Engineering Station and the SIMATIC IOT2050 to establish a SSH connection an Ethernet cable is required.|
|**UART cable** (optional, but recommended)|To establish a serial connection to the IOT2050 in order to get into the u-boot shell a 3.3V USB-UART cable is needed. There are many hardware possibilities, good experiences were made with [this cable](https://de.rs-online.com/web/p/entwicklungstool-zubehor/0429307/).|
|**DisplayPort Cable (Male-Male) and Monitor**|If you would like to have local connection to the SIMATIC IOT2050, you need to have DisplayPort Cable, a monitor that supports DisplayPort. Alternatively, an **active** DP-HDMI converter may be used.|
|**Keyboard**|If you would like to have local connection to the SIMATIC IOT2050, you need to have a keyboard connected to IOT2050.|
|**Power supply**|In order to run the SIMATIC IOT2050 a power supply is required. This power supply has to provide between 12 and 24V DC.|

___

## **Required Software**

This chapter contains the software required for the setting up.

|**Software**|**Description**|
|-|-|
|**Example Image**|To use the full functionality of the SIMATIC IOT2050 a SD-Card Example Image with a Debian based Linux Operating System is necessary to be installed. This Image is provided through the Siemens Industry Online Support. The Example Image can be downloaded from the [IOT2050 Download Page](https://support.industry.siemens.com/cs/document/109741799/downloads-for-simatic-iot20x0).|
|**ssh client**|Remote access to the SIMATIC IOT2050 requires appropriate software. We recommend using [MobaXterm](https://mobaxterm.mobatek.net/), which also supports file transfers via drag and drop  from the Engineering Station to the IOT2050. (Instead of MobaXterm you also can use Windows or Linux built-in ssh client - the files can also be transfered to the device using a USB flash drive)|
|**Win32 Disk Imager**|In order to put the SD Card image to the µSD Card, software is needed. In this Setting Up the Win32 Disk Imager is used. The Win32 Disk Imager can be downloaded at [sourceforge.net](https://sourceforge.net/projects/win32diskimager/).|

![Download-Example-Image](/docs/graphics/1-Download-Example-Image.png)

> We always recommend using the latest version of Example Image V1.5.x.

___

## **Operating**

This chapter describes the steps necessary to install and start up the SIMATIC IOT2050 using the hard- and software listed above

### **Installing the SD-Card Example Image**

The first step to work with the SIMATIC IOT2050 is to set up a Micro-SD Card with the image provided through the Siemens Industry Online Support.

|No.|Action|
|-|-|
|1.|Insert the µSD-Card via SD-Card Adapter in the SD-Card Slot of your Engineering Station|
|2.| Retrieve the downloaded SD Card image `iot2050-image-example-iot2050-debian-iot2050.wic.img`|
|3.|Install the downloaded `Win32DiskImager-x.x.x-install.exe`|
|4.|Start the Win32 Disk Imager|
|5.|Select the Example Image file and your targed SD card device and click on `Write`|
||![Write-Image](/docs/graphics/1-Write-Image.png)|
|6.|Confirm the warning message (All data will be deleted!) and wait for the `Write Successful` message|
|7.|Right click on `Safely Remove Hardware and Eject Media` and choose your SD card|
|8.|Open the card cover on the bottom of the IOT2050 and insert the µSD-Card into the µSD-Card Slot of the SIMATIC IOT2050|
||![Insert-SD-card](/docs/graphics/1-iot2050-insert-sd-card.png)|

___

### **Power Supply of the SIMATIC IOT2050**

This chapter shows how to connect the SIMATIC IOT2050 to a power supply.

|No.|Action|
|-|-|
|1.|Power off the power supply|
|2.|Connect the cable to the connecting terminal|
|3.|Connect the connecting terminal to the SIMATIC IOT2050|
|4.|Power on the power supply|
||![Power-Supply](/docs/graphics/1-iot2050-power-supply.png)|

> **CAUTION:** Use only a DC power supply rated between 12 and 24 volts!

___

### **Local access**

The following table shows how to connect the SIMATIC IOT2050 using a DisplayPort supported monitor via DisplayPort cable and a keyboard.

>The IOT2050SM does not support DisplayPort connection!

|No.|Action|
|-|-|
|1.|Connect one end of the DisplayPort cable to a Display-Port of the monitor|
|2.|Connect the other end of the DisplayPort cable to the Display-Port of the SIMATIC IOT2050.|
|3.|Connect a keyboard to USB port of SIMATIC IOT2050|

> The initial boot may take up to two minutes due to automatic filesystem resizing. The time is depending on the SD card you are using.

___

### **Remote ethernet access with SSH**

The following table shows how to connect the SIMATIC IOT2050 and the engineering station with an Ethernet cable

|No.|Action|
|-|-|
|1.|Connect one end of the Ethernet cable to an Ethernet-Port of the Engineering Station|
|2.|Connect the other end of the Ethernet cable to the Ethernet-Port X1P1 of the SIMATIC IOT2050.|

> By default, the SIMATIC IOT2050 is configured with a static IP address. This address is `192.168.200.1`. The Engineering Station has to be in the same subnet as the SIMATIC IOT2050
to establish a SSH connection!

The following table shows how to use MobaXterm to establish a ssh connection to the IOT2050.

|No.|Action|
|-|-|
|1.|Open downloaded and installed MobaXterm application|
|2.|Select `Session` > Choose `SSH` >  Enter the remote host IP address `192.168.200.1` > Specify username `root` > Select port `22` > Confirm with `OK`|
||![MobaXterm-Configuration](/docs/graphics/1-MobaXterm-config.png)|
|3.|If neccessary accept the new server hostkey before connecting to the device (first time connecting via SSH)|
||![MobaXterm-Hostkey](/docs/graphics/1-MobaXterm-hostkey.png)|
|4.|Log in with the default password `root` (username was specified in the configuration in step 2)|
||![MobaXterm-Login](/docs/graphics/1-MobaXterm-login.png)|
|5.|You will be prompted to change your password for the user `root` > Set & Confirm a new password|
||![MobaXterm-Change-Password](/docs/graphics/1-MobaXterm-change-password.png)|

___

### **Remote serial access with UART connection**

A UART cable is a very helpful device because you can establish a serial connection via MobaXterm and interrupt the boot. This can be helpful in many cases:

- To change boot order permanently
- To select to boot from SD card / USB only for the upcoming boot
- To connect to a system serially instead of using ssh (e.g. IP address is not known and there is no monitor)
- Detect the problem, when IOT2050 does not boot for some reasons

|No.|Action|
|-|-|
|1.|Power off the IOT2050|
|2.|Connect the UART cable to the IOT2050 via the ``X14 interface``. Therefore it is required to open the top housing for the Arduino interface to access ``X14``. The ``M wire`` (black in this example) needs to be connected to the ``pin 1``|
||![IOT2050-UART-Connection](/docs/graphics/1-iot2050-uart-connection.png)|
|3.|Connect the USB part of the cable to your PC. Drivers may need to be installed, please check the website of the vendor of the used cable.|
|4.|Go to Device Manager of your PC and check the assigned COM port. **NOTE**: If there is no COM port assigned and the device appears as an unknown device, it is needed to install the drivers for the cable|
||![Serial-COM-Port](/docs/graphics/1-Serial-COM-Port.png)|
|5.|Open downloaded and installed MobaXterm application|
|6.|Select `Session` > Choose `Serial` > Choose Serial port `COM3` (can differ!) > Specify Speed `115200bps` > Confirm with `OK`|
||![MobaXterm-Configuration-Serial](/docs/graphics/1-MobaXterm-config-serial.png)|
|7.|You can now log in using your `username` and `password`. For further actions at the very first boot, see section [Remote ethernet access with SSH](#remote-ethernet-access-with-ssh)|

___

### **User administration**

The following table shows how to create a new user and add them to `sudo group`

|No.|Action|
|-|-|
|1.|To create a new user, type ``adduser <username>`` and follow the prompts|
||![MobaXterm-Add-User](/docs/graphics/1-MobaXterm-adduser.png)|
|2.|You can add the user to `sudo group` by typing `adduser siemens sudo` (adjust the username to your needs)|
||![MobaXterm-Add-User-Sudo](/docs/graphics/1-MobaXterm-adduser-sudo.png)|

___

### **Install software packages on the device**

Provided example image includes apt package manager so that by using apt package manager new software can be installed on SIMATIC IOT2050. The following table shows how to install new software packages on the SIMATIC IOT2050.

> To access new software packages a internet connection is required on the device.

|No.|Action|
|-|-|
|1.|Open a valid serial connection and log in as root|
|2.|Before installing any software package, update repositories by typing ``apt update``|
||![Apt-Update](/docs/graphics/1-apt-update.png)|
|3.|To install a software package, use the command ``apt install <nameofsoftware>`` For example: ``apt install wireshark`` – it is a software to track network packages.|
||![Apt-Install](/docs/graphics/1-apt-install.png)|
|4.|To completely remove an installed software package with its configuration file type ``apt purge <nameofsoftware>`` For example: ``apt purge wireshark``|
||![Apt-Purge](/docs/graphics/1-apt-purge.png)|

___

### **Change boot order in Example Image**

The IOT2050 Advanced has an internal eMMc, which is set at first boot device by some functional states of the device. More information about the FS (Functional State) can be [found here](https://support.industry.siemens.com/cs/document/109989723/iot2050's-factory-firmware-versions-for-different-fs-number).

|No.|Action|
|-|-|
|1.|Open a valid serial connection and log in as root|
|2.|To check the current boot order the command ``fw_printenv boot_targets`` can be used:|
||![Check-Boot-Targets](/docs/graphics/1-check-boot-targets.png)|
||**Note:** **mmc1** = eMMc / **mmc0** = SD card / **usbx** = USB slots|
|3.|To change the boot order the command ``fw_setenv boot_targets "<devices>"`` can be used. **It is important to set the devices in quotes!** This is an example to have the external boot devices prior to the internal eMMc: ``fw_setenv boot_targets "mmc0 usb0 usb1 usb2 mmc1"``|
||![Set-Boot-Targets](/docs/graphics/1-set-boot-targets.png)|
|4.|To check whether this was successful, call ``fw_printenv boot_targets`` again:|
||![Check-Boot-Targets-After](/docs/graphics/1-check-boot-targets-after.png)|

> When using the Example Images of version V1.0.2 and V1.1.1, the quotation marks ("") within the ``fw_setenv-command`` must be omitted, e.g. ``fw_setenv boot_targets mmc0 usb0 usb1 usb2 mmc1``

___

### **Change boot order in in u-boot shell via UART connection**

The UART connection may be used to enter the u-boot shell and modify the boot sequence or select a specific boot device for the next boot. How to establish a UART connection see chapter [Remote serial access with UART connection](#remote-serial-access-with-uart-connection).

|No.|Action|
|-|-|
|1.|Open a valid serial connection and log in as root|
|2.|Interrupt the boot process at the point ``Hit SPACE to stop autoboot in x seconds...``. This will end up in the u-boot shell (indicated by ``=>`` or ``IOT2050>``)|
||![U-Boot-Interrupt](/docs/graphics/1-u-boot-interrupt.png)|
|3.|To check the current boot order the command ``print boot_targets`` can be used:|
||![Check-Boot-Targets](/docs/graphics/1-u-boot-check-boot-targets.png)|
||**Note:** **mmc1** = eMMc / **mmc0** = SD card / **usbx** = USB slots|
|4.|To change the boot order the command ``setenv boot_targets "<devices>"`` can be used. **It is important to set the devices in quotes!** This is an example to have the external boot devices prior to the internal eMMc: ``setenv boot_targets "mmc0 mmc1 usb0 usb1 usb2"``. The changes need to be saved by ``saveenv``.|
||![Set-Boot-Targets](/docs/graphics/1-u-boot-set-boot-targets.png)|
|4.|To check whether this was successful, call ``print boot_targets`` again:|
||![Check-Boot-Targets-After](/docs/graphics/1-u-boot-check-boot-targets-after.png)|
|5.|Type in ``boot`` to continue booting with the changed boot order.|

___

### **Skip eMMc as boot device**

To use the Example Image with the IOT2050 Advanced it is possible to neglect/skip the eMMc as boot device and only check external devices for bootable images.

|No.|Action|
|-|-|
|1.|Press and hold the USER button|
|2.|Power on / Reset the IOT2050 Advanced|
|3.|Keep the USER button pressed until the STAT LED turns orange|
||![Skip-eMMc](/docs/graphics/1-skip-emmc.png)|
|4.|Release the USER button|
|5.|IOT2050 is booting only from external media|

___

### **Status LED's using Example Image**

The IOT2050 variants have four different Status LEDs: PWR, STAT, USER1 & USER2.

|LED|Status|Indication|
|-|-|-|
|PWR (Power) LED|Green|Indicates that the IOT is in operation.|
|STAT (Status) LED|Green|System is starting up.|
||Green blinking|The operating system is running.|
||Red|System startup failed or no valid operating system is detected.|
||Red blinking|The operating system crashed.|
||Orange|EIO controller firmware update needed. See section|
|USER1 & USER2 LED||These are user-programmable LEDs and can be set to indicate different statuses (Red, Green, Orange) or functions as defined by the user.|

A running operating system contains a green PWR-LED and a green-blinking STAT-LED.

___

### **Basics of IOT2050-Setup Tool**

The IOT2050 Example Image comes with an pre-installed IOT2050-Setup Tool which can be accessed with the command `iot2050setup` as root user.

![IOT2050Setup-Overview](/docs/graphics/1-iot2050setup-overview.png)

|Main Menu|Sub Menu|Description|
|-|-|-|
|OS Settings|Change Hostname|Allows you to update the host name of your IOT2050 device, by default it's set to `iot2050-debian`|
||Change Password|Allows you to update the login password using the command ``passwd``|
||Change Time Zone|Allows you to select a specific time zone based on your geographical area|
|Networking|Edit a connection|Allows you to edit network parameters of existing network connections and to add / delete network connections. By default X1 is set to static ip-address `192.168.200.1` and X2 to DHCP.|
||Activate a connection|Allows you to De-/Activate existing network connections|
||Set system hostname|Allows you to update the host name of your IOT2050 device, by default it's set to `iot2050-debian`|
|Software|Manage Autostart Options|Update the Autostart option of preinstalled services like SSH Server, TCF Agent, Mosquitto Broker & Node-RED|
|Peripherals|Configure External COM Ports|Switch between available modes for the external COM port: RS232 / RS485 / RS422|
||Configure Arduino I/O|Configure Arduino Pinout / Enable GPIO / Enable I2C / Enable SPI / Enable UART / Enable PWM / Enable ADC|
||Configure M.2 Connector|Advanced Configurations for M.2 Connector|

> Some adjustments need a power cycle of the device to take effect!

___

### **EIO controller firmware update**

If the STAT LED of your device remains orange and you receive the following brodcast message an EIO controller firmware update is necessary.

![IOT2050-EIO-Firmware-Brodcast](/docs/graphics/1-eio-brodcast-message.png)

|No.|Action|
|-|-|
|1.|Open a valid serial connection and log in as root.|
|2.|Use the command `iot2050-eio fwu controller` to perform an eio firmware update.|

___

### **IOT2050SM WebUI**

The new IOT2050SM Industrial gateway is designed to connect to S7-1200 SM modules. For the device configuration at least Example Image V1.4.x is required as it includes the ``EIO-configuration-interface`` to configure the connected modules.

|No.|Action|
|-|-|
|1.|Connect your selected and supported S7-modules to your IOT2050 SM using the IO expansion cable.|
||![IOT2050SM-Connecting](/docs/graphics/1-iot2050sm-connecting.png)|
|2.|Power on your IOT2050 SM and open a valid ssh connection and log in as root.|
|3.|To access the ``EIO-configuration-interface``, open a browser on your PC connected to the IOT2050, open the URL ``http://<IP of the IOT2050>:2050/`` and click on the sub-menu ``EIO Config``.|
||![IOT2050SM-Access-UI](/docs/graphics/1-iot2050sm-access-interface.png)|
|4.|Configure the different slots regarding your needs and click on Deploy.|
||![IOT2050SM-Configure-Deploy](/docs/graphics/1-iot2050sm-configure-deploy.png)|
||![IOT2050SM-Deploy-OK](/docs/graphics/1-iot2050sm-deploy-ok.png)|
|5.|After a few seconds the connected modules should switch into running mode.|
|6.|The module data will then be stored in different files e.g. ``value_raw`` in the directory ``/eiofs/controller/slot1/``.|
||![IOT2050SM-EIOFS](/docs/graphics/1-iot2050sm-eiofs.png)|

___

### **Restore IOT2050 Example Image**

Restore manual

## **Appendix**

### **Service and support**

**SiePortal**
The integrated platform for product selection, purchasing and support - and connection of Industry Mall and Online support. The SiePortal home page replaces the previous home pages of the Industry Mall and the Online Support Portal (SIOS) and combines them.

- Products & Services: In Products & Services, you can find all our offerings as previously available in Mall Catalog.
- Support: In Support, you can find all information helpful for resolving technical issues with our products.
- mySieportal: mySiePortal collects all your personal data and processes, from your account to current orders, service requests and more. You can only see the full range of functions here after you have logged in.

You can access SiePortal via this address: [sieportal.siemens.com](https://sieportal.siemens.com/en-ww/home).

**Technical Support**
The Technical Support of Siemens Industry provides you fast and competent support regarding all technical queries with numerous tailor-made offers – ranging from basic support to individual support contracts.
Please send queries to Technical Support via Web form: [support.industry.siemens.com/cs/my/src](https://support.industry.siemens.com/cs/my/src?lc=en-WW)

**SITRAIN – Digital Industry Academy**
We support you with our globally available training courses for industry with practical experience, innovative learning methods and a concept that’s tailored to the customer’s specific needs.
For more information on our offered trainings and courses, as well as their locations and dates, refer to our web page: [siemens.com/sitrain](https://www.siemens.com/sitrain)

**Industry Online Support app**
You will receive optimum support wherever you are with the "Industry Online Support" app. The app is available for iOS and Android:

|![Available-App-Store](/docs/graphics/0-Available-App-Store.png)|![Available-Google-Store](/docs/graphics/0-Available-Google-Store.png)|
|-|-|
|![QR-Apple](/docs/graphics/0-QR-Apple.png)|![QR-Google](/docs/graphics/0-QR-Google.png)|

### **Links & Literature**

|No.|Topic|
|-|-|
|1|[Siemens Industry Online Support](https://support.industry.siemens.com/)|
|2|[IOT2050 Forum](https://sieportal.siemens.com/en-ww/support/forum/topics/309)|
|3|[IOT2050 Download Page](https://support.industry.siemens.com/cs/ww/en/view/109780231)|
|4|[IOT2050 Operating Instructions](https://support.industry.siemens.com/cs/ww/en/view/109779016)|
|5|[How To Set Up IOT2050 with Example Image](https://support.industry.siemens.com/tf/ww/en/posts/238945/)|

## **Contribution and Contribution License Agreement**

Thank you for your interest in contributing. Anybody is free to report bugs, unclear documentation, and other problems regarding this repository in the Issues section.
Additionally everybody is free to propose any changes to this repository using Pull Requests.

If you haven't previously signed the [Siemens Contributor License Agreement](https://cla-assistant.io/industrial-edge/) (CLA), the system will automatically prompt you to do so when you submit your Pull Request. This can be conveniently done through the CLA Assistant's online platform. Once the CLA is signed, your Pull Request will automatically be cleared and made ready for merging if all other test stages succeed.

## **Licence and Legal Information**

Please read the [Legal information](/LICENSE).
