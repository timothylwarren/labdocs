



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
