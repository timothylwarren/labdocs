



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
	`sudo ntpd -gq`
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

###alternative method to set time zone
- sudo timedatectl set-timezone US/Pacific-New
#list of available time zones 
- timedatectl list-timezones #shows 1 time zone per line, scroll to find best fit

- sudo raspi-config
- sudo nano /boot/config.txt (add 'dtoverlay=i2c-rtc,ds3231' to end of the file)
- sudo reboot
#log in again ('nm-connection-editor', and then 'ssh pi@10.42.0.###')
- sudo service ntp stop
- sudo ntpd -gq
- sudo service ntp start

- sudo apt-get update
- sudo apt-get install i2c-tools #may need to add '--fix-missing' to command
- sudo apt-get install python-smbus i2c-tools

###- sudo apt-get -y remove fake-hwclock
###- sudo update-rc.d -f fake-hwclock remove
###- sudo systemctl disable fake-hwclock
- timedatectl #check that local time is correct
- date #check time that RTC time to write into pi is correct
###- sudo nano /lib/udev/hwclock-set
- sudo hwclock -r #read hardwire clock on pi
- sudo hwclock -w #write new time onto hardwire clock on pi
- sudo hwclock -r #check that time is now correct
#enter password for pi
sudo nano /lib/udev/hwclock-set

###manually setting clock time (refer https://www.freedesktop.org/software/systemd/man/timedatectl.html)
- sudo timedatectl set-local-rtc 0 
- sudo timedatectl set-time '2021-10-14 13:32:00' #use next full minute time and enter when clock on computer changes to set time within ~1 sec of real time)
#confirm time and date are correct using 
- timedatectl

#if error message "Failed to set time: Automatic time synchronization is enabled"
- 'sudo timedatectl set-ntp 0'
#then redo steps above to reset clock manually

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

# How to install Sublime (https://linuxize.com/post/how-to-install-sublime-text-3-on-ubuntu-20-04/)

