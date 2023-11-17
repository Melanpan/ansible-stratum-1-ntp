# Ansible Playbook To Deploy A GPS-baesd Stratum 1 NTP server
Note: This playbook has been designed for NurdSpace.

Made to be run on Raspbian 64bit lite, make sure it's installed on a >=8gb SD.

# Features
- log2ram
- chrony (ntp pools configured to be an NL pool)
- prometheus exporters
- pps via a GPIO pin

# Deploy
Make sure you fill out the appropiate hosts in inventory and setup the host vars for said hosts. 
Needed variables are:

* pps_gpio_pin (The pin where the GPS's pps signal is connected to)
* gpsd_options (GPSD options see man)
* gpsd_devices (The devices gpsd uses, e.g /dev/ttyAMA0 /dev/pps0)
* gpsd_serial_device (The serial device where the GPS module is on)
* log2ram_size (How much MB log2ram keeps in memory before flushing)

Once everything has been deployed, you can check if everything works by running these commands:
#### sudo ppstest /dev/pps0
```
ok, found 1 source(s), now start fetching data...
source 0 - assert 1700218111.877897677, sequence: 81 - clear  0.000000000, sequence: 0
```

#### gpsmon
Make sure it sees satalites and that you see text containing "PPS OFFSET" scrolling by

#### log2ram
`sudo systemctl status log2ram`

#### chrony
```
nurds@timepi:~ $ chronyc sources
MS Name/IP address         Stratum Poll Reach LastRx Last sample
===============================================================================
#* PPS                           0   4    17    19    +29ns[+1996ns] +/-  412ns                       0   4     3    14   -216ms[ -216ms] +/-  564ns
```

*This playbook will reboot the device a couple of times.
