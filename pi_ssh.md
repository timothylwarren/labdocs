



## List of steps to connect pi from Ubuntu without internet

Tim, Abheer 5.20.21

[link to helpful page](https://raspberrypi.stackexchange.com/questions/3867/ssh-to-rpi-without-a-network-connection)

Key steps were:
- `sudo apt-get install dnsmasq-base`
- `nm-connection-editor` then opens GUI, where you set up a new ethernet connection
- Find ip of pi (not sure if we can do that reliably) `cat /var/lib/misc/dnsmasq.leases` worked on one linux machine but not the other
- Then ssh pi@10.42.0.198 (password:raspberry)
- on Terminal: `cd /Documents/RPi_Cam_Web_Interface` 
- `./start.sh` on one computer `./update.sh` might be needed beforehand
- navigate to browser entering ip address

5.25.21

-how to control settings such as time lapse on rpi cam interface
	-`timelapse start`, `timelapse stop`, starts/stops automated images
	-'Camera settings' to modify lapse, intervals.
	-seems to save to /var/www/html/media on pi


06.08.21
## testing presence of rtc on pi

- timedatectl (will give error if rtc is not equipped)
- useful link for setting RTC time on pi (https://learn.adafruit.com/adding-a-real-time-clock-to-raspberry-pi/set-rtc-time)
- link containing all the timedatectl commmands (https://www.freedesktop.org/software/systemd/man/timedatectl.html)
- link containing instructions for ntp synchronization (https://domoticproject.com/keeping-raspberry-on-time-rtc-module/#DS3231_Module_Setup)

-ntp synchronization:
	`sudo service ntp stop`
	`ntpd -gq`
	`sudo service ntp start`
-rtc synchronization with ntp
	`sudo hwclock -w`
	`sudo hwclock -r`
	
-useful link (http://www.intellamech.com/RaspberryPi-projects/rpi_RTCds3231#4) 
	


