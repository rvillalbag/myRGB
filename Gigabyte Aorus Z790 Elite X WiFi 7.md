# Gigabyte Aorus Z790 Elite X WiFi 7
VID: 0x048D, PID: 0x5702

** Thank you to [OpenRGB](https://gitlab.com/CalcProgrammer1/OpenRGB) for providing clues to this and to Wireshark and USBPcap for facilitating the means for doing the reverse engineering.<br>

Used USB communication to control the RGB via API calls to libusb-1.0.so<br>

Using Wireshark USBPcap I discovered Gigabyte Control Center (GCC) sending the following set of 64-byte USB packets via a Control Transfer to the device when attempting to set the RGB.<br>
> cc200100000000000000000164000064ff00000000000000b004c800c8000000010...0<br>
> cc210200000000000000000164000064ff00000000000000b004c800c8000000010...0<br>
> cc220400000000000000000164000064ff00000000000000b004c800c8000000010...0<br>
> cc230800000000000000000164000064ff00000000000000b004c800c8000000010...0<br>
> cc241000000000000000000164000064ff00000000000000b004c800c8000000010...0<br>
> cc252000000000000000000164000064ff00000000000000b004c800c8000000010...0<br>
> cc264000000000000000000164000064ff00000000000000b004c800c8000000010...0<br>
> cc278000000000000000000164000064ff00000000000000b004c800c8000000010...0<br>
> cc28ff0000000000000000000000000000000000000000000000000000000000000...0<br><br>

> cc200100000000000000000164000064ff00000000000000b004c800c8000000010...0<br>
> cc210200000000000000000164000064ff00000000000000b004c800c8000000010...0<br>
> cc220400000000000000000164000064ff00000000000000b004c800c8000000010...0<br>
> cc230800000000000000000164000064ff00000000000000b004c800c8000000010...0<br>
> cc241000000000000000000164000064ff00000000000000b004c800c8000000010...0<br>
> cc252000000000000000000164000064ff00000000000000b004c800c8000000010...0<br>
> cc264000000000000000000164000064ff00000000000000b004c800c8000000010...0<br>
> cc278000000000000000000164000064ff00000000000000b004c800c8000000010...0<br>
> cc28ff0000000000000000000000000000000000000000000000000000000000000...0<br><br>

GCC appears to send the same 9 blocks twice.  Each block set seems to end with the same cc28ff block which I suspect is some kind of done/commit block.  No idea why they are sent twice.<br>

The 8 blocks common to both sets seem to place the RGB values in bytes 16 (red), 15 (green) and 14 (blue).

Note: One of these bytes is also for mode but I only cared about the static mode so I do not have any information on what values and what bytes to place mode-specific information.<br>

Note: I was only concerned with setting the onboard motherboard RGB, these messages also affect the RGB header pins - I did not do any testing to determine which of the 8 blocks were specific to the onboard LEDs vs RGB headers.

