# Munin plugin: ipmitool\_sensor_

ipmitool\_sensor_ is a wildcard-plugin to monitor sensors by using ipmitool.

## Install and Configuration

The possible wildcard values are the following: fan, temp, volt. So you would create symlinks to this plugin called ipmitool\_sensor\_fan, ipmitool\_sensor\_temp, and ipmitool\_sensor_volt.

    ln -s /usr/share/munin/plugins/ipmitool_sensor_ /etc/munin/plugins/ipmitool_sensor_fan
    ln -s /usr/share/munin/plugins/ipmitool_sensor_ /etc/munin/plugins/ipmitool_sensor_temp
    ln -s /usr/share/munin/plugins/ipmitool_sensor_ /etc/munin/plugins/ipmitool_sensor_volt

Add the following to your /etc/munin/plugin-conf.d/munin-node:

Note: When you use this, the threshold provided by the sensor board is not used.

    [ipmitool_sensor*]
        user root
        timeout 20

If you want to use "ipmitool sdr", add the following:

    [ipmitool_sensor*]
        user root
        timeout 20
        env.ipmitool_options sdr

## Configurable variables

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

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## Copyright

* Copyright (C) 2008 - 2013 Jun Futagawa (jfut)
* License
  * GPLv2

