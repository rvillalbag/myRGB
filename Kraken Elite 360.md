# Kraken Elite 360
VID: 0x1E71, PID: 0x300C

** Thank you to Wireshark and USBPcap for facilitating the means for doing the reverse engineering.

This is the device that kickstarted the whole project for me as NZXT only provides NZXT CAM for Windows 10/11 to control the LCD display.  I reached out to them to find out if there were any plans to add support for Linux but they said no.<br>

Used USB communication to control the LCD via API calls to libusb-1.0.so<br>

Using Wireshark USBPcap I discovered NZXT CAM sending all kinds of messages, I will upload a couple of the Wireshark sessions.<br>

The messaging breaks down into three stages: Startup, Running and Shutdown.  All messages are 64-bytes in size, padded with zeros.  The value after TX/RX in braces is the USB interface used 01h=Interrupt Write, 02h=Bulk Write and 81h=Interrupt Read. The messaging pattern for the most part is that repsonses +1 to the first byte of the request message.<br>

# Startup
No idea what all these messages do but they were consistently sent on each startup of the NZXT CAM software.<br>

> TX(01): 10 02<br>
> RX(81): 11 02<br>
> TX(01): 70 02 01 b8 0b<br>
> RX(81): ff 01<br>
> TX(01): 74 01<br>
> RX(81): 75 01<br>
> TX(01): 36 04<br>
> RX(81): 37 04<br>
> TX(01): 30 01<br>
> RX(81): 31 01<br>
> TX(01): 36 03<br>
> RX(81): 37 03<br>
> TX(01): 30 02 00 00 00 00 00 00 1e<br>
> RX(81): ff 01<br>
> TX(01): 38 01 02<br>
> RX(81): 39 01<br>
> TX(01): 32 02 00<br> RX(81): 33 02<br>
> TX(01): 32 02 01<br> RX(81): 33 02<br>
> TX(01): 32 02 02<br> RX(81): 33 02<br>
> TX(01): 32 02 03<br> RX(81): 33 02<br>
> TX(01): 32 02 04<br> RX(81): 33 02<br>
> TX(01): 32 02 05<br> RX(81): 33 02<br>
> TX(01): 32 02 06<br> RX(81): 33 02<br>
> TX(01): 32 02 07<br> RX(81): 33 02<br>
> TX(01): 32 02 08<br> RX(81): 33 02<br>
> TX(01): 32 02 09<br> RX(81): 33 02<br>
> TX(01): 32 02 0a<br> RX(81): 33 02<br>
> TX(01): 32 02 0b<br> RX(81): 33 02<br>
> TX(01): 32 02 0c<br> RX(81): 33 02<br>
> TX(01): 32 02 0d<br> RX(81): 33 02<br>
> TX(01): 32 02 0e<br> RX(81): 33 02<br>
> TX(01): 32 02 0r<br> RX(81): 33 02<br>
> TX(01): 30 02 01 64 00 00 00 00 1e<br>
> RX(81): ff 01<br>

# Running
The running stage is a 1 second loop where in each loop a fresh copy of the image was transmitted to the LCD display and one or more status (0x7502) messages were received.<br>

## Image
The image data to be sent is 640 lines of 640 pixels of RGB data, so 640 x 640 x 3 = 1,228,800 raw bytes.  And in my case, based on the physical installation of the pump required a 90 degree rotation for the image to appear vertical.<br>

Example image being rendered and sent to the LCD display:<br>
![Example Image](https://raw.githubusercontent.com/2ndage/myRGB/refs/heads/main/Kraken%20LCD.bmp)

## Image Updating
> TX(01): 36 01 00 01 09<br>
> RX(81): 37 01<br>
> TX(02): 12 fa 01 e8 ab cd ef 98 76 54 32 10 09 00 00 00 00 c0 12<br>
> TX(02): # IMG DATA<br>
> TX(01): 36 02<br>
> RX(81): 37 02<br>
> TX(01): 72 01 01 00 46 ..41x.. 46  # Set Pump Duty 46h=70% <br>
> RX(81): ff 01<br>
> TX(01): 72 02 01 01 32 ..41x.. 32  # Set Fan Duty 32h=50%<br>
> RX(81): ff 01<br>

## Cooler Monitoring
Periodically an unsolicited response would be received of type 0x7502 which, thanks to [liquidctl](https://github.com/liquidctl/liquidctl) provides the cooler's summary information.<br> 
> RX(81): 75 02 ... # Cooler Information<br>
> - Liquid Temperature: Byte{15} + (Byte{16} / 10)<br>
> - Pump Duty: Byte{19}<br>
> - Pump Speed: (Byte{18} << 8) | Byte{17}<br>
> - Fan Duty: Byte{25}<br>
> - Fan Speed: (Byte{24} << 8) | Byte{23}<br>

# Shutdown
The shutdown, although not seemingly mandatory, immediately reverts the LCD display to its factory default of displaying the liquid temperature and NZXT name in white text.<br>

> TX(01): 38 01 02<br>
> RX(81): 39 01<br>
