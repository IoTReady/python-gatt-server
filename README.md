# Python BLE GATT Server (peripheral)
GATT is constructed out of one or more server devices (BLE peripherals) and a client device (BLE central).

A GATT server is usually a small device such as a sensor, but for some use cases you might want to have a Linux computer such as a RPi used as a GATT server. This example is meant to demonstrate how this can be done.

Forked from: https://github.com/Jumperr-labs/python-gatt-server

## Installation

Original README (see below) includes instructions for Ubuntu. For Arch Linux:

    sudo pacman -S bluez python-gobject gobject-introspection python-cairo
    sudo systemctl enable bluetooth
    sudo systemctl start bluetooth
    cd python-gatt-server
    python3 -m venv venv
    source venv/bin/activate
    pip install PyGObject
    python gatt_server_example.py

## How To Use

- Edit gatt_server.py and change dummy services and characteristics. 
    - See, for instance, https://github.com/IoTReady/python-gatt-server/blob/master/gatt_server.py#L44 to disable services
    - And, https://github.com/IoTReady/python-gatt-server/blob/master/gatt_server.py#L425 to edit a service
- Run gatt_server_example.py which imports gatt_server and runs a loop
- Restart bluetooth service every time you kill the script - otherwise advertisement kept failing for me.
- Test with nRF Connect mobile app

    sudo systemctl restart bluetooth

## To Do

- ~~Fork repo~~
- ~~Remove heartbeat and battery services~~ and code
- Create a sample service and 5-6 characteristics for easy customisation
    - Remove the encrypted and secure characteristics unless needed.

---
# Original Instructions Follow

## Setup
The instructions in this document were tested on Ubuntu 16.04.

### Bluez Installation
On most distributions, you will need to upgrade Bluez in order to make this work.

Install dependencies:
```bash
sudo apt-get update
sudo apt-get install libudev-dev libical-dev libreadline-dev libglib2.0-dev libdbus-1-dev
```

Download and install Bluez

```bash
wget http://www.kernel.org/pub/linux/bluetooth/bluez-5.45.tar.xz
tar xvf bluez-5.45.tar.xz
cd bluez-5.45/
./configure
make
sudo make install
```

Enable experimental features for the bluetooth driver: 
- `sudo nano /lib/systemd/system/bluetooth.service`
- Add `--experimental` to `ExecStart`. The line should look like this: 

    `ExecStart=/usr/local/libexec/bluetooth/bluetoothd --experimental`

Reload the service:
```bash
systemctl daemon-reload
sudo service bluetooth restart
```

### virtualenv
Install dependencies: `sudo apt-get install virtualenv python-dev libdbus-1-dev libdbus-glib-1-dev python-gi`

`cd` to the the root of this repository and:

```bash
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
ln -s /usr/lib/python2.7/dist-packages/gi venv/lib/python2.7/site-packages/
```

## Usage
Start the sample BLE GATT server: `python gatt_server_example.py`

You can use a smartphone as a GATT client. I used the [GATT-IP](http://www.gatt-ip.org/) app, here it is on the [Google Play Store](https://play.google.com/store/apps/details?id=org.gatt_ip.activity&hl=en) and on the [App Store](https://itunes.apple.com/us/app/gatt-ip-bluetooth-smart-le-proxy-protocol/id940105344?mt=8)

![](http://jumper-public.s3-website.eu-central-1.amazonaws.com/gatt-ip.gif)

## License
The code in this repository is based on code taken from the [BlueZ](http://www.bluez.org/) project. It is licensed under GPL 2.0
