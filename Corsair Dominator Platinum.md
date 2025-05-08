# Corsair Dominator Platinum
VID: 0x8086, PID: 0x7A23<br>

** Thank you to [OpenRGB](https://gitlab.com/CalcProgrammer1/OpenRGB) for providing clues to this.<br>

Used SMBUS communication to control the RGB.<br>

Scanned the SMBUS I801 adapter for addresses from 0x18 to 0x1F and 0x58 to 0x5F to locate the memory modules by doing a test IOCTL write to the I2C address with Command=0 and Size=0.<br>

Of the addresses that responded, proceeded to perform a byte read on command 0x43 and if the response was 0x1A or 0x1B proceeded to perform a byte read on command 0x44 and if the response was 0x04 determined the correct memory module was found.<br>

Of the verified addresses: 0x19 and 0x1b, proceeded to send block writes to set the RGB.

Created a 38 byte array (12 LEDs * 3 RGB + 2 overhead).<br>
- Byte 0: Set to 12 (number of LEDs)
- Byte 1-36: RGB values for each of the 12 LEDs.
- Byte 37: CRC-8 value.

Sent the data over SMBUS using two I2C Block Writes with a 1ms sleep between writes.
- The first write sent the first 32 bytes of the 38 total bytes using command 0x31.
- The second write sent the last 6 bytes using command 0x32.

