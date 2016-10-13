# Troubleshooting and FAQs
This page provides answers to common questions and solutions to common problems.

## Reviving a board that will not boot
If your Snapdragon Flight board does not boot up, you may be able to recover it with a special-purpose serial console debug board. Use the following procedure to recover the Snapdragon Flight board:
1. Disconnect power and USB cable(s) from the Snapdragon Flight and debug boards.
2. Plug the Snapdragon Flight board into the debug board.
3. Enable (insert jumper) J2 (FASTBOOT) of the debug board.
4. Connect the debug board’s J4 to the computer’s USB port, using the provided USB cable, and open a serial terminal (CuteCom or Putty).
5. Power up the debug board.
6. Connect the Snapdragon Flight board to the computer via a USB cable.
7. Validate the connection by typing “fastboot devices” on a command prompt or terminal window. You should now see the device-specific random number displayed.
8. You should be able to flash the platform software images at this point.

To boot the board normally again, perform the following steps:
1. Unplug the power supply and USB cable.
2. Remove the jumper from J2 on the serial console debug board.
3. Power up the Snapdragon Flight board and connect the board to the computer via a USB cable.
4. Validate the connection by typing “adb devices” on a command prompt or terminal window.

