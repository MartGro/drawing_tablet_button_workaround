# Problem statement
I recently bought a XP-PEN Star G640 drawing tablet. 
On my Ubuntu 16.04 machine, the DIGImend drivers for non-Wacom drawing tablets do not work (they cause a black screen during boot). Luckily, even without installing any driver, the tablet almost works. The only problem is that the rocking button is not recognized correctly: Pressing the lower button leads to a middle mouse click (which is OK), but the upper button maps to a left click. This means it has the same behavior as "clicking" with the stylus itself.

The problem is that with this configuration you end up with basically one button. For really using the tablet, you need two: one for erasing and one for moving around. 


__TLDR__: G640 Tablet has one wrong keymapping on the upper rocking button: Left mouse click instead of right mouse click.

# Idea
Look at the input of the tablet and change every left click that comes from pressing the upper rocking button to a right click.

# Done so far: 

## How does the tablet input things

__DISCLAIMER: I don't really have an idea what I'm doing. Everything is learning by doing.__

The tablet is a USB device that writes to __/dev/input/event{number}__.

#### Reading the input:
One way of reading the input is by the command-line program *evtest*.
The problem is that __/dev/input/event{number}__ is restricted for the normal user. Since we need access to this, we need to make the tablet readable to the user. 


#### Making the tablet input accessible for the user:

The principle for this is outlined in the following stackoverflow post: 

https://stackoverflow.com/questions/13220566/linux-raw-input-without-root-permission

1. We create a suitable udev rule: The file is called **99-tablet.rules**. The directory is __/etc/udev/rules.d/__. Note: The exact filename is not important, as long as it ends in *.rules* . Creating this file requires __root__ privileges, but only one time.

'''plaintext
ATTRS{idVendor}=="28bd", ATTRS{idProduct}=="0914", MODE="664", GROUP="tablet"
'''


2. 
