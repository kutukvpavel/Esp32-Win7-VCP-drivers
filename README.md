# ESP32 Win7 VCP drivers
INF files that enable ESP32-S3 Serial/JTAG peripheral and TinyUSB CDC stack operation as VCP on Win7/8.

The issue with Win7 and Win8.1 is that usbser.sys class driver doesn't get installed automatically for devices belonging to the Communications Device Class (CDC), because Windows 7/8 lacks usbser.inf and possibly some other components I don't have the expertise to fiddle with. Therefore, even if you install WinUSB driver as per Espressif documentation (https://docs.espressif.com/projects/esp-idf/en/v4.3/esp32s2/api-guides/dfu.html?#api-guide-dfu-flash-win), you won't be able to get a serial port (only a useless "Universal Serial Bus Device" will show up).
This is well-documented on MS website:
 - https://docs.microsoft.com/en-us/windows-hardware/drivers/usbcon/usb-driver-installation-based-on-compatible-ids
 - https://docs.microsoft.com/en-us/troubleshoot/windows-client/deployment/how-to-use-reference-usbser-driver-universal-serial-bus
 - https://docs.microsoft.com/en-us/windows-hardware/drivers/usbcon/supported-usb-classes

Therefore, the solution is simple: provide your own INF file with your VID/PID that references usbser.sys in some way. MS recommends to do it indirectly, through modem class. You don't have to install WinUSB using Zadig, if you use these custom INFs! If you did, you may need to uninstall it. Windows will warn you that it can't verify the publisher of these INFs, since I didn't include a \*.cat file, you can ignore it.

This repo contains INFs with VID/PID/MIs for:
 - ESP32-S3 embedded Serial/JTAG USB stack (located in ROM), activated as "(secondary) serial console" in configuration \[esp32-vcp-jtag.inf\]
 - ESP32-IDF TinyUSB stack, configured to use Espressif VID and TinyUSB PID \[esp32-tinyusb.inf\]

In case your particular chip has a different PID, you can just edit the INF yourself (look at \[Models. ...\] sections).

Thanks to David Grayson for an INF template: https://stackoverflow.com/questions/41928144/inf-file-cant-find-usbser-sys-in-windows-7-only
