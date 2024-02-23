# rpi3-gameboy
A Raspberry Pi 3B+ based handheld games console.

# Capabilities
Can play retro games from multiple consoles upto the Playstation 1. This includes: Nintendo Systems (NES, SNES, GB, GBA, GBC), Sega Systems (Master System, Mega Drive) & the Playstation 1. Most systems before the PS1 should work. All thanks to the Retropie OS. Go to [Retropie Supported Systems](https://retropie.org.uk/docs/Supported-Systems/) page for more info.

# Build
### Electronics
Microcontroller Board - Raspberry Pi 3B+  
Battery Pack - [2S 7.4V 3000mAh LiPo](https://www.aliexpress.com/item/1005005797608566.html)  
Display - [ILI9341 3.2inch 320x240 SPI TFT](https://www.aliexpress.com/item/1005003120684423.html)  
Input - [Gameboy Zero PCB Button Board](https://www.aliexpress.com/item/32993474020.html)  
Buttons - [Original Gameboy Rubber Pads and Buttons](https://www.aliexpress.com/item/1005002518901690.html)  
Power Converter - [LM2596 Step Down Converter 3A](https://www.aliexpress.com/item/32653212622.html)  

### Code
Retropie handles the emulation part of the project. Code is needed to get the **button board** and the **display** to work.  
#### Button board
For the button board I used the [GPIOnext](https://github.com/mholgatem/GPIOnext) library. 
First, SSH into your pi, then
```
cd ~

git clone https://github.com/mholgatem/GPIOnext.git
```
Next run the script,
```
bash GPIOnext/install.sh
```
Enter to run configuration manager, select Joypad 1, the script will then ask for how many DPADs are connected, select 1 DPAD.  
Then select the following options (START, SELECT, A, B, X, Y, L, R), these represent the buttons present on the joypad.  
The script will then run through each button, press each the button as they pop up in the script.  
Then save and exit.  
Finally, 
```
gpionext start
```
And then restart your pi, the library handles starting up with the pi so don't worry about that.

If you mess up some of the controls, use
```
gpionext config
```
to remap them. This will take you through the same process again.

#### Display
For the display I used the  [fbcp-ili9341](https://github.com/juj/fbcp-ili9341} library.
First, SSH into your pi, then type the following commands
```
cd ~
git clone https://github.com/juj/fbcp-ili9341.git
sudo apt-get install cmake
cd fbcp-ili9341
mkdir build
cd build
make -j
sudo ./fbcp-ili9341
cmake -DILI9341=ON -DGPIO_TFT_DATA_CONTROL=24 -DGPIO_TFT_RESET_PIN=25 -DSPI_BUS_CLOCK_DIVISOR=6 -DSTATISTICS=0 -DDISPLAY_ROTATE_180_DEGREES=ON  ..
```
To make the script to run on startup, change directory to /etc/ and type the following command to edit the file,  
```
nano rc.local
```
Then add the following line before the exit 0 line.
```
sudo /home/pi/fbcp-ili9341/build/fbcp-ili9341 &
```
The LCD panel should start after rebooting the pi.  
Now change directory to /boot/ folder and type the following command to edit the file,
```
nano config.txt
```
Then, uncomment the following line,
```
hdmi_force_hotplug=1
```
Uncomment the following line to disable overscan,
```
disable_overscan=1
```
Add the following lines to set the custom resolution,
```
hdmi_cvt=320 240 60 5
hdmi_group=2
hdmi_mode=87
hdmi_drive=2
```
That's all the code needed to get this project to work.
### CAD Modelling

### Final Build

# Photos

# Things to improve
