# Intro
Thinkfan is a service that lets you manually define the thinkpad's fan speeds at different temps


# Kernel module
Install the `thinkpad_acpi` kernel module from repos

## Kernel module features
The kernle module needed some nudging:
```
echo "options thinkpad_acpi fan_control=1 experimental=1" | sudo tee /etc/modprobe.d/thinkfan.conf
sudo modprobe -rv thinkpad_acpi
sudo modprobe -v thinkpad_acpi
```

# Deamon
A service - `thinkfan` - is required to run to make the changes. Install it from the repos.

## Set up service
Recently (in 21.04), thinkfan switched to using a `.yaml` file to setup the service that is hard to configure as there's no examples online. A temporary solution is to continue using the old `.conf` file for the time being. A vanilla example of the old .conf file can be found here: https://github.com/vmatare/thinkfan/blob/0.9/examples/thinkfan.conf.simple.

### Add .conf file

```
sudo rm /etc/thinkpad.yaml
sudo mv ~/Downloads/thinkfan.conf.simple /etc/thinkfan.conf
```

### Add temp sensors
Add temp sensors to the conf file with this one-liner:
```
find /sys/devices -type f -name 'temp*_input' | xargs -I {} echo "hwmon {}" >> /etc/thinkfan.conf
```
Note that most of the sensors it adds are going to change between reboots, causing the service to fail. Remove all the sensor entries except for those starting with `hwmon /sys/devices/pci0000:00/0000:00:1b.0/0000:02:00.0/` (the device id might be different for you).

### Make sure fan is registered
Conf file should contain `tp_fan /proc/acpi/ibm/fan`

### Create your fan profile
Create a fan profile for machine here - ` /etc/thinkfan.conf`. The one I used was like this:

```
(0,     0,      60)
(1,     55,     65)
(2,     63,     68)
(3,     67,     72)
(4,     70,     75)
(5,     75,     80)
(7,     77,     93)
(127,   83,     32767)
```

### Fix crashes
The service could crash because of restarts, sleep states and so on. To fix it edit the service file:
`sudo nano thinkfan.service`
and add to the service section:
`Restart=on-failure RestartSec=10s`

### A note on multi fan support
Thinkfan does not support multifan setups, but the way the `thinkpad_acpi` driver is implemented exposes only one fan control that actually controlls both fans. This means that this solution will probably work for you.

