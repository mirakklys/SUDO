# SUDO

***

## Pop_OS!

***

#### show usb devices

`lsusb`

#### update software, install GParted and NeoFetch

`sudo apt-get update && sudo apt-get upgrade -y && sudo apt-get install neofetch gparted inxi ffmpeg jq npm mpv inkscape gnome-tweaks mypaint htop vlc blender flatpak gparted -y`

`flatpak remote-add --user --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo`

#### tiktok-scraper installation

`npm i --package-lock-only`

`sudo npm i -g tiktok-scraper`

#### sublime text 3

`wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add - && sudo apt-get install apt-transport-https && echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list && sudo apt-get update && sudo apt-get install sublime-text -y`

#### install pip and all needed packages for comfortable pythoning

`sudo pip install pillow && sudo pip install jupyterlab && sudo pip install voila`

#### set the time to GMT so that the time of Win machine is not messed up

`timedatectl set-local-rtc 1 --adjust-system-clock`

#### show kernel in use

`uname -mrs`

#### recover Win bootloader

    lsblk -o NAME,FSTYPE,SIZE,MOUNTPOINT
        NAME          FSTYPE   SIZE MOUNTPOINT
            nvme0n1              238.5G 
            ├─nvme0n1p1   vfat     260M                         // =============>	This is Win EFI bootloader	
            ├─nvme0n1p2             16M 
            ├─nvme0n1p3   ntfs   159.1G 
            ├─nvme0n1p4   ntfs    1000M 
            ├─nvme0n1p5   vfat     512M /boot/efi               // =============>	This is Pop_OS! EFI bootloader
            ├─nvme0n1p6   swap       4G 
            │ └─cryptswap swap       4G [SWAP]
            └─nvme0n1p7   ext4    73.6G /
            nvme1n1              238.5G 
            ├─nvme1n1p1             16M 
            └─nvme1n1p2   ntfs   238.5G /media/radmir/Новый том
    sudo mkdir /mnt/win-efi
    sudo mount /dev/nvme0n1p1 /mnt/win-efi                      // mount Win efi into /mnt/win-ef
    sudo cp -r /mnt/win-efi/EFI/Microsoft /boot/efi/EFI         // copy Win efi files into Pop_OS! efi bootloader
    sudo micro /boot/efi/loader/loader.conf                     // add timer to systemd-boot
        ex:	timeout=10
    sudo reboot

#### find empty directories

`find . -type d -empty`

#### find a file containing "foo" as start

`find . -name "foo*"`

#### install chrome

`wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb && sudo apt install ./google-chrome-stable_current_amd64.deb`

#### install drivers for the wifi module

`sudo apt-get update && sudo apt-get install dkms git && git clone https://github.com/brektrou/rtl8821CU.git && cd ~/rtl8821CU && sudo ./dkms-install.sh && sudo /usr/sbin/usb_modeswitch -KW -v 0bda -p 1a2b && sudo modprobe 8821cu`

#### extract the command history

`$ARCHIVE_OF_USED_COMMANDS`

***

## Fedora

`sudo dnf upgrade`

***

## Windows

#### set the style of command line

`setx PROMPT $T$H$H$H$S$G$S$P$_$$$S`

#### turn off BDE encryption

`manage-bde -status`

`manage-bde -off <drive letter>:`

***

## ffmpeg commands

#### show all available devices

`ffmpeg -list_devices true -f dshow -i dummy`

#### record screen

`ffmpeg -rtbufsize 1500M -f dshow -i audio="Microphone (Realtek(R) Audio)" f -y -rtbufsize 100M -f gdigrab -framerate 30 -probesize 10M -draw_mouse 1 -i desktop -c:v libx264 -r 30 -preset ultrafast -tune zerolatency -crf 10 -pix_fmt yuv420p testing.mp4`

#### trim the ends of the file

`ffmpeg -ss 0:00:00 -to 1:23:45 -i file_name.mp4 -vcodec libx264 -crf 18 testing.mp4`

#### trim XX sec from the end

`ffmpeg -t XX -i file_name.mp4 -vcodec libx264 -crf 18 testing.mp4`

#### concatenate few files together

`ffmpeg -i input1.mp4 -i input2.webm -i input3.mov filter_complex "[0:v:0][0:a:0][1:v:0][1:a:0][2:v:0][2:a:0]concat=n=3:v=1:a=1[outv][outa]"  -map "[outv]" -map "[outa]" -vcodec libx264 -crf 18 testing.mp4`

#### write all source devices into separate files

`ffmpeg -f x11grab -framerate 60 -video_size 1920x1080 -i :0.0 f v4l2 -framerate 30 -i /dev/video0 -f alsa -ac 2 -ar 44100 -i pulse -f pulse -ac 2 -i alsa_output.pci-0000_01_00.1.hdmi-stereo.monitor -map 0:v:0 x11.mp4 -map 1:v:0 webcam.mp4 -map 2:a:0 -acodec libfdk_aac mic.aac -map 3:a:0 -acodec libfdk_aac desktop_audio.aac`

#### streaming screen to file or stream service (mic included)

`ffmpeg -f x11grab -framerate 30 -i :1.0 -f pulse -i default -c:v libx264 -preset fast -crf 26 -maxrate 2500k -bufsize 5000k -g 60 -vf format=yuv420p -c:a aac -b:a 128k -f flv stream.mp4`

###### Examples of using mapping and concat functions

`ffmpeg -ss 6:35 -to 6:45 -i GMT20220201-085454_Recording_1920x1080.mp4 -c:v libx264 -crf 18 1.mp4 && ffmpeg -ss 7:25 -i GMT20220201-085454_Recording_1920x1080.mp4 -c:v libx264 -crf 18 2.mp4 && ffmpeg -i 1.mp4 -i 2.mp4 -filter_complex "[0:v:0][0:a:0][1:v:0][1:a:0]concat=n=2:v=1:a=1[outv][outa]" -map "[outv]" -map "[outa]" BIOL471_Feb_1.mp4 && rm -rf 1.mp4 2.mp4 GMT20220201-085454_Recording_1920x1080.mp4`

`ffmpeg -ss 6:35 -to 6:45 -i GMT20220201-085454_Recording_1920x1080.mp4 -c:v libx264 -crf 18 1.mp4 && echo "file 1.mp4" > file.txt && ffmpeg -ss 7:25 -i GMT20220201-085454_Recording_1920x1080.mp4 -c:v libx264 -crf 18 2.mp4 && echo "file 2.mp4" >> file.txt && ffmpeg -f concat -safe 0 -i file.txt BIOL471_Feb_1.mp4 && rm -rf 1.mp4 2.mp4 file.txt GMT20220201-085454_Recording_1920x1080.mp4`

`ffmpeg -ss 10:23 -to 25:35 -i GMT20220202-055744_Recording_1920x1080.mp4 -c:v libx264 -crf 18 1.mp4 && ffmpeg -ss 26:00 -to 42:03 -i GMT20220202-055744_Recording_1920x1080.mp4 -c:v libx264 -crf 18 2.mp4 && ffmpeg -i 1.mp4 -i 2.mp4 -filter_complex "[0:v:0][0:a:0][1:v:0][1:a:0]concat=n=2:v=1:a=1[outv][outa]" -map "[outv]" -map "[outa]" -vcodec libx264 -crf 18 MBI_Feb_2_1.mp4 && rm -rf 1.mp4 2.mp4 && ffmpeg -ss 47:47 -to 1:55:14 -i GMT20220202-055744_Recording_1920x1080.mp4 -c:v libx264 -crf 18 MBI_Feb_2_2.mp4 && ffmpeg -ss 1:58:51 -to 2:49:31 -i GMT20220202-055744_Recording_1920x1080.mp4 -c:v libx264 -crf 18 1.mp4 && ffmpeg -ss 2:53:01 -i GMT20220202-055744_Recording_1920x1080.mp4 -c:v libx264 -crf 18 2.mp4 && ffmpeg -i 1.mp4 -i 2.mp4 -filter_complex "[0:v:0][0:a:0][1:v:0][1:a:0]concat=n=2:v=1:a=1[outv][outa]" -map "[outv]" -map "[outa]" MBI_Feb_2_3.mp4 && rm -rf 1.mp4 2.mp4 GMT20220202-055744_Recording_1920x1080.mp4`

`ffmpeg -ss 0:23 -to 10:05 -i GMT20220202-055744_Recording_1920x1080.mp4 -c:v libx264 -crf 18 1.mp4 && ffmpeg -ss 10:23 -to 15:35 -i GMT20220202-055744_Recording_1920x1080.mp4 -c:v libx264 -crf 18 2.mp4 && ffmpeg -ss 26:03 -to 33:33 -i GMT20220202-055744_Recording_1920x1080.mp4 -c:v libx264 -crf 18 3.mp4 && ffmpeg -ss 37:00 -to 42:03 -i GMT20220202-055744_Recording_1920x1080.mp4 -c:v libx264 -crf 18 4.mp4 && for i in {1..4} ; do echo "file $i.mp4" >> file.txt ; done && ffmpeg -f concat -safe 0 -i file.txt BIOL471_Feb_1.mp4 && for i in {1..4} ; do rm -f $i.mp4 ; done && rm -f file.txt GMT20220202-055744_Recording_1920x1080.mp4`
