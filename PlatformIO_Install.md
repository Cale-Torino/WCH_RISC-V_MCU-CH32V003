# PlatformIO_Install

How I setup my  VSCode PlatformIO environment for the CH32V003 IC's.

## Install VSCode

Make sure VSCode is installed

Download and install VSCode from https://code.visualstudio.com/.

## Install PlatformIO

[<img src="name" width="500"/>](name)

| Condition  |  Voltage | Typical Current  | Minimum Current  |
| :------------ | :------------ | :------------ | :------------ | 
|CLK_CPU=`20MHz` (OSC20M)      |VDD=5V  |9.0mA  |- 

## Install CH32V Platform

Expand the PlatformIO sidebar (ant icon) and click 'PIO Home'.

[<img src="name" width="500"/>](name)

In the resulting PIO Home window, click on the “Platforms” sidebar and chose “Advanced Installation”

[<img src="name" width="500"/>](name)

You will be asked for a repistory. Enter the URL and press “Install”.

```
https://github.com/Community-PIO-CH32V/platform-ch32v.git
```

[<img src="name" width="500"/>](name)

If you get the error:

```
Could not install platform
PIO Core Call Error: "Platform Manager: Installing git+https://github.com/Community-PIO-CH32V/platform-ch32v.git\r\n\n\nUserSideException: Please install Git client from https://git-scm.com/downloads"
```

Install the Git client from:

```
https://git-scm.com/downloads
```

```
https://github.com/platformio/platformio-core/issues/2811
```

[<img src="name" width="500"/>](name)

The platform should now be successfully installed.

Experienced [PlatformIO CLI](https://docs.platformio.org/en/latest/integration/ide/vscode.html#platformio-core-cli) users can also use the short-hand command.

```
pio pkg install -g -p https://github.com/Community-PIO-CH32V/platform-ch32v.git
```

## Install Drivers / Rules

### Windows Driver Installation

Flashing development boards via a WCH-Link(E) probe (and SWCLK and/or SWDIO connection) requires that W.CH’s USB drivers for that are installed.

- Download the [WCHLink Driver Windows](https://github.com/Community-PIO-CH32V/wchlink-driver-windows/archive/refs/heads/main.zip) package
- Unpack it
- Run `WCHLink\SETUP.EXE` and follow installation instructions
- Run `WCHLinkSER\SETUP.EXE` and follow installation instructions

If successful, once you plug in the WCH-Link(E) device, you should have a “serial port” and “interface”-type device in the Windows device manager.

[<img src="name" width="500"/>](name)

Flashing development boards via their built-in USB bootloader requires that WinUSB drivers are loaded for that device.

- Download [Zadig](https://zadig.akeo.ie)
- Start Zadig and activate Options → List All Devices
- Put your development board into bootloader mode
    - for most: BOOT0 to 3.3V, BOOT1 to GND
    - or hold marked “BOOT” button
- Plug development board into computer
- Select the “USB Module” device in Zadig
- Select “WinUSB” on the right side
- Click “Replace driver”

It should now look like this:

[<img src="name" width="500"/>](name)






Links:
- [32-bit general-purpose RISC-V MCU-CH32V003](https://www.wch-ic.com/products/CH32V003.html)
- [Arduino Core for CH32V003](https://github.com/AlexanderMandera/arduino-wch32v003)
- [CH32 RISC-V MCUS GET OFFICIAL ARDUINO SUPPORT](https://hackaday.com/2024/01/04/ch32-risc-v-mcus-get-official-arduino-support/)
- [Official Arduino core support for CH32 EVT Boards](https://github.com/openwch/arduino_core_ch32)
- [Arduino Core for WCH CH32V](https://github.com/Community-PIO-CH32V/ArduinoCore-CH32V)
- [WCH Official Store](https://wchofficialstore.aliexpress.com)
- [About WCH](https://www.wch-ic.com/about_us.html)
- [CH32V003 RISC-V Mini Game Console](https://github.com/wagiminator/CH32V003-GameConsole)
- [lcsc CH32V003](https://www.lcsc.com/search?q=CH32V003)
- [mounriver](http://www.mounriver.com/)
- []()
- []()
- []()

