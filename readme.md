# Ansible Playbook To Deploy A GPS-baesd Stratum 1 NTP server
Note: This playbook has been designed for NurdSpace.

Made to be run on Raspbian 64bit lite, make sure it's installed on a >=8gb SD.

# Features
- log2ram
- ntpsec (configured to use the ntp.time.nl pool)
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

#### ntpsec
```
nurds@timepi:~ $ ntpq -c pe -n
     remote                                   refid      st t when poll reach   delay   offset   jitter
=======================================================================================================
*SHM(1)                                  .PPS.            0 l    8   64  357   0.0000  48.9037  64.4054
+SHM(0)                                  .GPS.            0 l    7   64  377   0.0000 -30.7004  30.7751
 ntp.time.nl                             .POOL.          16 p    -  256    0   0.0000   0.0000   0.0005
...
```

*This playbook will reboot the device a couple of times.
