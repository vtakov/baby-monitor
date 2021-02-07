# Baby-monitor
Raspberry pi driven baby monitor

## Setup
In this project we use VS1838B IR receiver for remote control and first we need the LIRC package in order to decode infrared signals
### Install LIRC
```
sudo apt-get install lirc liblircclient-dev
```

### Set LIRC
~~Run the following command to edit /etc/modules~~
~~sudo  nano /etc/modules~~

~~Add the following lines in /etc/modules~~
~~lirc_dev~~
~~lirc_rpi gpio_in_pin=18 gpio_out_pin=17~~

Run the following command to edit /boot/config.txt
```
sudo  nano  /boot/config.txt
```

Add the follow line at the end of the file under the # Enable audio (loads snd_bcm2835)
```
dtoverlay=gpio-ir,gpio_out_pin=17,gpio_in_pin=18,gpio_in_pull=up
```

Copy the config files
```
sudo cp ./lirc-conf/* /etc/lirc/
```

Edit /etc/lirc/lirc_options.conf and change this settings to:
```
driver = default
device = /dev/lirc0
```

**reboot**

### Check if everything works

After login you should have a lirc0 device:
```
ls -l /dev/lirc0
```

Check services with:
```
systemctl status lircd.service
systemctl status lircd.socket
```

Now we can look if we get the pushed buttons.
Start irw and push buttons on your remote control.
```
$ irw
0000000000002422 00 KEY_VOLUMEUP Sony_RMT-CS33AD
0000000000006422 00 KEY_VOLUMEDOWN Sony_RMT-CS33AD
```

Last step is to give these events actions, e.g. start a program. For this we use the program irexec.
This needs its config file ~/.lircrc
Create a new one or copy the `.lircrc` file from the src folder
