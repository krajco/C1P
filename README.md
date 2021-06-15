
define command{

    command_name    check_magnetic_status
    command_line    $USER1$/check_oid.py -H $HOSTADDRESS$ -o $ARG1$ -c $ARG2$ -im Closed! -wm 'Warning doors are opened (s)' -cm Closed -ec 300 -gw 60
}

define command{

    command_name    check_sensor_battery
    command_line    $USER1$/check_oid.py -H $HOSTADDRESS$ -o $ARG1$ -c $ARG2$ -im 'Battery OK' -wm 'Battery is low' -cm 'Battery is verry low' -lw 30 -lc 15
}

define command{

    command_name    check_sensor_motion
    command_line    $USER1$/check_oid.py -H $HOSTADDRESS$ -o $ARG1$ -c $ARG2$ -im 'No motion' -wm 'Motion detected before(s)' -lw 1800
}

define command{

    command_name    check_shp6_rssi
    command_line    $USER1$/check_oid.py -H $HOSTADDRESS$ -o $ARG1$ -c $ARG2$ -im 'Signal OK' -wm 'Signal is low' -cm 'Signal is very low' -lw 50 -lc 25
}

define command{

    command_name    check_shp6_uptime
    command_line    $USER1$/check_oid.py -H $HOSTADDRESS$ -o $ARG1$ -c $ARG2$ -im 'Uptime is OK' -wm 'Uptime is high' -cm 'Uptime is critical' -gw 8640000 -gc 12960000
}

define command{

    command_name    check_shp6_power
    command_line    $USER1$/check_oid.py -H $HOSTADDRESS$ -o $ARG1$ -c $ARG2$ -im 'Voltage is OK' -wm 'Voltage is higher' -cm 'Voltage is critical' -gw 240 -gc 250
}

define command{

    command_name    check_air_temperature
    command_line    $USER1$/check_oid.py -H $HOSTADDRESS$ -o $ARG1$ -c $ARG2$ -im 'Temperature is OK' -wm 'Temperature is higher' -cm 'Temperature is critical' -gw 26 -gc 30
}
