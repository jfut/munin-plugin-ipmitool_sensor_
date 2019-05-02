# Munin plugin: ipmitool\_sensor_

ipmitool\_sensor_ is a wildcard-plugin to monitor sensors by using ipmitool.

## Install and Configuration

The possible wildcard values are the following: fan, temp, volt. So you would create symlinks to this plugin called ipmitool\_sensor\_fan, ipmitool\_sensor\_temp, and ipmitool\_sensor_volt.

```
ln -s /usr/share/munin/plugins/ipmitool_sensor_ /etc/munin/plugins/ipmitool_sensor_fan
ln -s /usr/share/munin/plugins/ipmitool_sensor_ /etc/munin/plugins/ipmitool_sensor_temp
ln -s /usr/share/munin/plugins/ipmitool_sensor_ /etc/munin/plugins/ipmitool_sensor_volt
```

Add the following to your /etc/munin/plugin-conf.d/munin-node:

Note: When you use this, the threshold provided by the sensor board is not used.

```
[ipmitool_sensor*]
    user root
    timeout 20
```

If you want to use "ipmitool sdr", add the following:

```
[ipmitool_sensor*]
    user root
    timeout 20
    env.ipmitool_options sdr
```

## Configurable variables

```
    ipmitool            - ipmitool command (default: ipmitool)
    ipmitool_options    - ipmitool command options (default: sensor)
                          sdr: you can use 'sdr' instead of sensor.
    cache_file          - cache file
                          (default: /var/lib/munin/plugin-state/plugin-ipmitool_sensor.cache)
    cache_expires       - cache expires (default: 275)
    
    fan_type_regex      - Regular expression for unit of fan (default: RPM)
    temp_type_regex     - Regular expression for unit of temp (default: degrees C)
    volt_type_regex     - Regular expression for unit of volt (default: (Volts|Watts|Amps))
    
    fan_warn_percent    - Percentage over mininum for warning (default: 5)
    fan_lower_critical  - Preferred lower critical value for fan
    fan_upper_critical  - Preferred upper critical value for fan
    temp_lower_critical - Preferred lower critical value for temp
    temp_lower_warning  - Preferred lower warining value for temp
    temp_upper_warning  - Preferred upper warning value for temp
    temp_upper_critical - Preferred upper critical value for temp
    volt_warn_percent   - Percentage over mininum/under maximum for warning
                          Narrow the voltage bracket by this. (default: 20)
```

## HOW TO TEST

```
  cache_file=ipmitool_sensor_ cache_expires=-1 ./ipmitool_sensor_fan
  cache_file=ipmitool_sensor_ cache_expires=-1 ./ipmitool_sensor_fan config
  cache_file=ipmitool_sensor_ cache_expires=-1 ./ipmitool_sensor_fan suggest
  cache_file=ipmitool_sensor_ cache_expires=-1 ./ipmitool_sensor_fan autoconf

  cache_file=ipmitool_sensor_ cache_expires=-1 ./ipmitool_sensor_fan config
  fan_warn_percent=50 fan_lower_critical=100 fan_upper_critical=1000 \
    cache_file=ipmitool_sensor_ cache_expires=-1 ./ipmitool_sensor_fan config

  cache_file=ipmitool_sensor_ cache_expires=-1 ./ipmitool_sensor_temp config
  temp_lower_warning=1 temp_lower_critical=2 temp_upper_critical=71 temp_upper_warning=72 \
    cache_file=ipmitool_sensor_ cache_expires=-1 ./ipmitool_sensor_temp config

  volt_warn_percent=50 \
    cache_file=ipmitool_sensor_ cache_expires=-1 ./ipmitool_sensor_volt config
```

### TEST DATA

```
  lnr  Lower Non-Recoverable
  lcr  Lower Critical
  lnc  Lower Non-Critical
  unc  Upper Non-Critical
  ucr  Upper Critical
  unr  Upper Non-Recoverable
```

### IPMITOOL SENSOR

```
  # HP ProLiant ML110 G5
  CPU FAN          | 1434.309   | RPM        | ok    | 5537.099  | 4960.317  | 4859.086  | na        | 937.383   | na
  SYSTEM FAN       | 1497.454   | RPM        | ok    | 5952.381  | 5668.934  | 5411.255  | na        | 937.383   | na
  System 12V       | 12.152     | Volts      | ok    | na        | na        | na        | na        | na        | na
  System 5V        | 5.078      | Volts      | ok    | na        | na        | na        | na        | na        | na
  System 3.3V      | 3.271      | Volts      | ok    | na        | na        | na        | na        | na        | na
  CPU0 Vcore       | 1.127      | Volts      | ok    | na        | na        | na        | na        | na        | na
  System 1.25V     | 1.254      | Volts      | ok    | na        | na        | na        | na        | na        | na
  System 1.8V      | 1.842      | Volts      | ok    | na        | na        | na        | na        | na        | na
  System 1.2V      | 1.107      | Volts      | ok    | na        | na        | na        | na        | na        | na
  CPU0 Diode       | na         | degrees C  | na    | na        | 20.000    | 25.000    | 85.000    | 90.000    | 95.000
  CPU0 Dmn 0 Temp  | 24.500     | degrees C  | ok    | na        | 0.000     | 0.000     | 97.000    | 100.000   | 100.500
  CPU0 Dmn 1 Temp  | 29.000     | degrees C  | ok    | na        | 0.000     | 0.000     | 97.000    | 100.000   | 100.500
  # HP ProLiant DL160
  FAN1 ROTOR1      | 7680.492   | RPM        | ok    | na        | inf       | na        | na        | 1000.400  | na
  # HP ProLiant DL360 G5
  Fan Block 1      | 34.888     | unspecified | nc    | na        | na        | 75.264    | na        | na        | na
  Fan Block 2      | 29.792     | unspecified | nc    | na        | na        | 75.264    | na        | na        | na
  Fan Block 3      | 37.240     | unspecified | nc    | na        | na        | 75.264    | na        | na        | na
  Fan Blocks       | 0.000      | unspecified | nc    | na        | na        | 0.000     | na        | na        | na
  Temp 1           | 40.000     | degrees C  | ok    | na        | na        | -64.000   | na        | na        | na
  Temp 2           | 21.000     | degrees C  | ok    | na        | na        | -64.000   | na        | na        | na
  Temp 3           | 30.000     | degrees C  | ok    | na        | na        | -64.000   | na        | na        | na
  Temp 4           | 30.000     | degrees C  | ok    | na        | na        | -64.000   | na        | na        | na
  Temp 5           | 28.000     | degrees C  | ok    | na        | na        | -64.000   | na        | na        | na
  Temp 6           | na         | degrees C  | na    | na        | na        | 32.000    | na        | na        | na
  Temp 7           | na         | degrees C  | na    | na        | na        | 32.000    | na        | na        | na
  Power Meter      | 214.000    | Watts      | cr    | na        | na        | 384.000   | na        | na        | na
  Power Meter 2    | 220.000    | watts      | cr    | na        | na        | 384.000   | na        | na        | na
  # HP ProLiant DL360 G7
  DL360 G7 Temp 1  | 25.000     | degrees C  | ok    | na        | na        | na        | na        | 42.000    | 46.000
  DL360 G7 Temp 2  | 40.000     | degrees C  | ok    | na        | na        | na        | na        | 82.000    | 83.000
  DL360 G7 Temp 3  | 40.000     | degrees C  | ok    | 10        | 5         | na        | na        | 82.000    | 83.000
```

### IPMITOOL SDR

```
  # HP ProLiant ML110 G5
  CPU FAN          | 1434.31 RPM       | ok
  SYSTEM FAN       | 1497.45 RPM       | ok
  System 12V       | 12.10 Volts       | ok
  System 5V        | 5.08 Volts        | ok
  System 3.3V      | 3.27 Volts        | ok
  CPU0 Vcore       | 1.14 Volts        | ok
  System 1.25V     | 1.25 Volts        | ok
  System 1.8V      | 1.84 Volts        | ok
  System 1.2V      | 1.11 Volts        | ok
  CPU0 Diode       | disabled          | ns
  CPU0 Dmn 0 Temp  | 23.50 degrees C   | ok
  CPU0 Dmn 1 Temp  | 29 degrees C      | ok
  # HP ProLiant DL360 G5
  Fan Block 1      | 34.89 unspecifi | nc
  Fan Block 2      | 29.79 unspecifi | nc
  Fan Block 3      | 37.24 unspecifi | nc
  Fan Blocks       | 0 unspecified     | nc
  Temp 1           | 41 degrees C      | ok
  Temp 2           | 19 degrees C      | ok
  Temp 3           | 30 degrees C      | ok
  Temp 4           | 30 degrees C      | ok
  Temp 5           | 26 degrees C      | ok
  Temp 6           | disabled          | ns
  Temp 7           | disabled          | ns
  Power Meter      | 208 Watts         | cr
  Power Meter 2    | 210 watts         | cr
```

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## Copyright

* Copyright (C) 2008 - 2019 Jun Futagawa (jfut)
* License
  * GPLv2

