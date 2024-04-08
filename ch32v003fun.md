# Installation

In general, for minichlink, the flashing/debugging utility, you can use the pre-compiled minichlink or go to minichlink dir and `make` it.

# Debian and WSL

```
apt-get install build-essential libnewlib-dev gcc-riscv64-unknown-elf libusb-1.0-0-dev libudev-dev gdb-multiarch
```
Note that gdb-multiarch is only needed/useful if you want to use gdb. You can still semihost/debug printf without gdb.

# Windows

Download and install (to system) [this copy](https://gnutoolchains.com/risc-v) of **GCC10**. The `minichlink.exe` file is already ready to go in the `minichlink` folder. It requires Microsoft Visual C++ Redistributable to be installed.

In Windows, if you want to use minichlink with the LinkE, you will need to use [Zadig](https://zadig.akeo.ie) to install WinUSB to the WCH-Link interface 0.

# WCH-Link (E)

It enumerates as 2 interfaces.

- 1. the programming interface. I can't get anything except the propreitary interface to work.
- 2. the built-in usb serial port. You can hook up UART D5=TX to RX and D6=RX to TX of the CH32V003 for printf/debugging, default speed is 115200. Both are optional, connect what you need.

If you want to mess with the programming code in Windows, you will have to install WinUSB to the interface 0. Then you can uninstall it in Device Manager under USB Devices.

On linux you find the serial port with `ls -l /dev/ttyUSB* /dev/ttyACM*` and connect to it with screen `/dev/ttyACM0 115200`
Disconnect with `CTRL+a` `:quit`.

Adding your user to these groups will remove the need to `sudo` for access to the serial port: debian-based `sudo usermod -a -G dialout $USER` arch-based `sudo usermod -a -G uucp $USER`

You'll need to log out and in to see the change.

Links:
- [Installation](https://github.com/cnlohr/ch32v003fun/wiki/Installation)
- []()
- []()
- []()

