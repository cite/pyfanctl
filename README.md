# pyfanctl

## CAUTION

This can fry your hardware. Use at your own risk!

## Overview

This is a very simple, very hacky way of controlling fans. I wrote this after
using a UGreen NASync DXP4800 and a LincPlus LincStation N2 and installed Debian
on both of them. I wanted more control over the fans, so I could spinup the fan
in the DXP4800 when the disks are running hot, even if the CPU is still cool.
And then I decided, why not increase idle cooling when I'm not at home.


## Requirements

Support for controlling fans and reading temperatures. In case of the DXP4800
and the N2, I had to enable the patched `it87` module from
[here](https://github.com/frankcrawford/it87). For the DXP4800, I also needed
the `drivetemp` module to check drive temperatures.


## Installation

* copy `pyfanctl` to `/usr/local/sbin/`
* copy `pyfanctl.service` to `/etc/systemd/system/`
* create a configuration file in `/etc/pyfanctl.json` (see templates)
* reload systemd, enable the service


## Dynamic Cooling

As seen in the configuration examples, three presets for cooling be defined
(well, must, if it's not defined but triggered, the script dies): `normal`,
`increased` and `silent`. The latter two are triggered by creating the files
`/tmp/increased_cooling` or `/tmp/silent_cooling`.


## Quick Primer

* `hwmon` drivers exported to `sysfs` live in `/sys/class/hwmon/hwmon*`
* each driver has a file called `name` that helps identifying it
* a driver can export more than one directory (e.g. `drivetemp`, one per drive)
* a fan is exported as a `pwm`, e.g. `/sys/class/hwmon/hwomX/pwm3`
* to switch from auto to manual fan control, write the value `1` into
  `pwmX_enable`
* to switch back, write `2`
