# Initial Boot Drive

Initially, I used balenaEtcher to make a boot drive containing Raspbian.

I touched a file named `ssh` in `/Volumes/boot` to make sure ssh was enabled on boot.

I also echoed the following file to `/Volumes/boot/wpa_sipplicant.conf` (leave quotes in):
```
country=US
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="<ssid>"
    scan_ssid=1
    psk="<pass>"
    key_mgmt=WPA-PSK
}
```

I then inserted the SD and booted the RPi.

# Connecting via SSH

I turned the RPi on my network and connected to it via `ssh pi@<ip>`.
The password is `raspberry`.

I then immediately daemonized ssh via
```
sudo systemctl enable ssh && sudo systemctl start ssh
```

I then modified the hostname to `donkeypi` by editing `/etc/hostname` to contain `donkeypi` and replacing all instances of the original hostname in `/ets/hosts` to be `donkeypi`.

I then re-connected via `ssh pi@donkeypi`.

# Installing Python3

Did all these commands

```
apt update && apt upgrade -y
apt install build-essential -y
apt install libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev -y
apt install zlib1g -y

cd /tmp

wget https://www.python.org/ftp/python/3.7.2/Python-3.7.2.tgz
cd Python-3.7.2
./configure --enable-optimizations
make
make install
```