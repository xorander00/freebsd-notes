# Bluetooth

## Connect
```sh
# Connect for playback only (works)
sudo virtual_oss -C 2 -c 2 -r 48000 -b 16 -s 1024  -R /dev/null -P /dev/bluetooth/00:00:00:00:00:00 -T /dev/sndstat -d dsp

# Connect for playback & recording (didn't work, complained about PSM)
sudo virtual_oss -C 2 -c 2 -r 48000 -b 16 -s 1024 -f /dev/bluetooth/00:00:00:00:00:00 -T /dev/sndstat -d dsp
```

## Prepare & Diagnostics
```shell
# Install packages
pkg install audio/virtual_oss

# Load kernel modules and install packages
kldload ng_ubt
kldload acpi_ibm

# Check sysctl variables (e.g. dev.acpi_ibm.0.bluetooth=1)
sysctl -a | grep -i "bluetooth"

# Touch config file as it is required to exist
touch /etc/bluetooth/bthidd.conf

# Start services
service bthidd start
service hcsecd start

# Pair device (put it into pairing mode & note PIN code if required by device)
bluetooth-config

# Create/check connection
hccontrol -n ubt0hci create_connection 00:00:00:00:00:00
hccontrol -n ubt0hci read_connection_list
```

------------------------------------------------------------------------------------------------------------------------

# Files

## /boot/loader.conf
```shell
acpi_ibm_load="YES"
ng_ubt_load="YES"
```

## /etc/rc.conf
```shell
bthidd_enable="YES"
hcsecd_enable="YES"
bluetooth_enable="YES"
```

## /etc/bluetooth/hosts
```shell
# Enter host->addr entries here to reference by name instead of by addr
```

## /etc/bluetooth/hcsecd.conf
```shell
device {
    bdaddr  00:00:00:00:00:00;
    name    "My BT Device";
    key     nokey;
    pin     "0000";
}
```
