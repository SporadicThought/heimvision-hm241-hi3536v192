## This document attempts to describe How to create a full flah ROM backup from hi3536dv197 board.
## I assume that most of the information conatined herein is compatible with hi3536v100 boards in general.
##
## 1.) Configure a tftp server on a local network machine. Set the ip of the tftp server to 192.168.1.99 (you may use a different ip/subnet if you set the 'serverip' and/or 'ipaddr' environment variables once you have gained access to the uBoot shell in step 5)
## 
## 2.) Connect to hi3536d PCB via UART interface (my pins were already soldered on). Do not connect power until ready to launch serial terminal as described in step 3. Note that UART is configured with PC Tx to the hi3536d Rx and the hi3536d Rx to the PC Tx to get readable output on the terminal.
## NOTE: I used the 3.3v UART pin via FTDI_USB<->TTY interface for power. I did not use the externel AC/DC adapter. I later discovered that when flashing back to the SPI on the board via HiTool that it is ideal to use the external AC/DC adapter and not to connect the 3.3v UART pin. I suggest that foregoing use of the 3.3v UART pin is best practice for downloading the flash as described in this article.
##

## 3. Launch a serial console. I prefer Tera Term on windows. I prefer the 'screen' command on linux "screen /dev/ttyUSB0 115200".
## Serial Console COM Port Settings:
## b115200 b8 pN s1 fN
## NOTE: if flow control is enabled you likely will not be able to send keyboard commands to the hi3536d
##
## 4. Power the hi3536d up. The console output will seem to bootloop and the board will emit a beep every few seconds. 
##
## 5. Press any key on your keyboard to gain access to the uBoot shell (the timing of your keypress has to be right; so it may take a couple keypresses)
##
## 6. Issue the following commands (not the lines prepended with ##):

hisilicon # sf probe 0

## 16384 KiB hi_fmc at 0:0 is now current device

hisilicon # mw.b 0x81000000 0xff 0x1000000
hisilicon # sf read 0x81000000 0 0x1000000
hisilicon # tftp 0x81000000 hi3536_spiflash_image_16M_STOCK_HEIMVISION_hm241_bak 0x1000000

## Hisilicon ETH net controler
## MAC:   6A-B8-62-36-6D-E3
## eth0 : phy status change : LINK=DOWN : DUPLEX=FULL : SPEED=100M
## eth0 : phy status change : LINK=UP : DUPLEX=FULL : SPEED=100M
## TFTP to server 192.168.10.99; our IP address is 192.168.10.10
## Upload Filename 'hi3536_spiflash_image_16M_STOCK_HEIMVISION_hm241_bak'.
## Upload from address: 0x81000000, 16.000 MB to be send ...
## Uploading: *#	[ Connected ]
## ################################	[ 2.888 MB]
## ################################	[ 5.752 MB]
## ################################	[ 8.616 MB]
## ################################	[11.480 MB]
## ############################
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#	[14.344 MB]
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
## %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
## 	 16.000 MB upload ok.

## That should do it. Verify that you have a valid 16M copy of the ROM in your server's tftp folder.
