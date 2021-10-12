



## List of steps to connect pi from Ubuntu without internet

Tim, Abheer, Cate, Kei 2021-10-11

[link to helpful page](https://raspberrypi.stackexchange.com/questions/3867/ssh-to-rpi-without-a-network-connection)

#Connect Pi to Ubuntu machine using ethernet cable
#Connect power source to Pi
#If you don't know the IP address of the Pi start here:
Key steps were:
- `sudo apt-get install dnsmasq-base`
- `nm-connection-editor` then opens GUI, where you set up a new ethernet connection
- Find ip of pi (not sure if we can do that reliably) `cat /var/lib/misc/dnsmasq.leases` worked on one linux machine but not the other

#Need to have only wired connection to Pi. Turn off connection to wireless and turn off airplane mode.
#You may need to toggle Pi connection on and off to get stable connection.
#If you know the Pi IP address use it and start here:
- Then ssh pi@10.42.0.198 (password:raspberry) #this is example of how to use IP address
- on Terminal: `cd /Documents/RPi_Cam_Web_Interface` 
- `./start.sh` on one computer `./update.sh` might be needed beforehand
- navigate to browser entering ip address

5.25.21

- how to control settings such as time lapse on rpi cam interface
	- `timelapse start`, `timelapse stop`, starts/stops automated images
	- 'Camera settings' to modify lapse, intervals.
	- seems to save to /var/www/html/media on pi


06.08.21
## testing presence of rtc (real time clock) on pi

- timedatectl #(will give error if rtc is not equipped)
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

10.11.21
## Need to Update for setting time

#set to Oregon time zone
#click tab for advanced settings - Pacific new time
#check this useful link (https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c)
#check this useful link (https://forums.raspberrypi.com/viewtopic.php?t=101267)
- sudo raspi-config
- sudo reboot
- sudo apt-get install -y i2c-tools 
#maybe you don't have to run these three commands
- sudo apt-get -y remove fake-hwclock
- sudo update-rc.d -f fake-hwclock remove
- sudo systemctl disable fake-hwclock

- sudo nano /lib/udev/hwclock-set
- sudo hwclock -r 
#enter password for pi

sudo nano /lib/udev/hwclock-set

## Starting virtual env for running python
- we followed this link (https://www.digitalocean.com/community/tutorials/how-to-set-up-jupyter-notebook-with-python-3-on-ubuntu-18-04)

- Key commands to activate and deactivate 
	`source kei_project_env/bin/activate`
	`deactivate`
	
- https://realpython.com/python-virtual-environments-a-primer/	

Set up another virtual environment, wind_env, on lab laptop
-Created wind_sensor_env directory and then from within that directory 'python3 -m venv wind_env' ,'
then 'source wind_env/bin/activate'

10.12.21
 'python3' #to start python within the environment
 
When activated `which python3` should point to location in virtual environment directory, not general python location

Sudo privileges *should not* be necessary for installations within virtual environment

## Permissions for reading usb file # do not do this within a virtual environment -- open new terminal
sudo chmod 777 '/dev/ttyACM0'

# to start wind sensor logger
# open directories for repositories and wind_sensor_env
'source wind_env/bin/activate' #opens open virtual environment
#using code from https://github.com/willdickson/m1_wind_sensor
'from m1_wind_sensor import M1Logger'
'logger = M1Logger(port='/dev/ttyACM0', filename='my_data.txt', window_size=120.0)'
'logger.run()'
#to stop logger 'Ctl c'
'quit()' 
'deactivate' #to quit python3


## How to set up Python3 on new linux machine

https://docs.python-guide.org/starting/install3/linux/

-`python3 --version`
-`sudo apt-get update`
-`sudo apt-get install python3.6`
-`pip install ipython` installs newest ipython that can be run with `ipython3` command


