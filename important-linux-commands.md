# Important Linux Commands

## Service
### Copy service, enable and Start it

```bash
sudo cp main.service /etc/systemd/system
sudo systemctl enable main.service
sudo systemctl daemon-reload
sudo systemctl restart main.service
```

### Follow Service Output
```bash
journalctl -f -u virtualhere.service
```

### Linux Service Format
```text
[Unit]
Description = System Service
#Requires

[Service]
User = pi
ExecStart =/home/pi/start.sh
RestartSec=5
Restart=always


[Install]
WantedBy = multi-user.target
```

## Fping Commands

```bash
sudo apt install fping || (echo "Error installing fping" && exit 1)
fping 192.168.1.101 
fping --timeout=50 < ip.txt
```

## Logging Shell Script

```bash
#!/bin/bash

sleep 5

LOG=/home/pi/logs/logs.txt 

timestamp() { 
date "+%b %d %Y %T %Z"
}

printf "\n\n" | tee -a $LOG
echo "$(timestamp): runtime started" | tee -a $LOG 
echo "-------------------------------------------------------------------------------" | tee -a $LOG 
cd /home/pi; 
python3 main.py;
echo "-------------------------------------------------------------------------------" | tee -a $LOG 
echo "$(timestamp): finished" | tee -a $LOG
printf "\n\n" | tee -a $LOG
```

## Setup Shell Script
```bash
#!/usr/bin/bash

set -e

if [ "$(id -u)" != "0" ]; then
echo "Current User : $USER"
echo "Please Run the Script as root user or with SUDO Priviledges" 1>&2
exit 1
fi

function error {
  echo -e "\\e[91m$1\\e[39m"
  exit 1
}


sudo apt update || error "Error Updating"
sudo apt-get install python3 || error "Error Install Python3"
sudo apt install python3-pip || error "Error Install pip"
/usr/bin/python3 -m pip --version
sudo apt install fping || error "Error installing fping"
/usr/bin/python3 -m pip install smbus2 || error "Error install pip-smbus2 Dependency"
/usr/bin/python3 -m pip install sht20 || error "Error install pip-sht20 Dependency"
/usr/bin/python3 -m pip install Flask || error "Error install pip-Flask Dependency"

echo "Dependency Install/Setup Successfull" && exit 0
```