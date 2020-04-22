# Home Assistant Automations and Configurations

## Raspberry Pi Setup with Hass OS

### Requirements

- Raspberry Pi (ZW/2/3/4)
- SD card (16/32GB)

### Installation

- Download [Home Assistant HassOS](https://www.home-assistant.io/hassio/installation/) image.

- Download [Balena Etcher](https://www.balena.io/etcher/) and run it to flash the HassOS image on SD card.

- Setup Wifi Connection
  - Open "hassos-boot" drive and make a new folder "CONFIG".
  - Open "CONFIG" and create a folder "network".
  - Open "network" and create a file "my-network".
  - Open my-network" with Notepad++ and paste the following configurations and save (replace SSID and WPAKEY).

```
[connection]
id=my-network
uuid=72111c67-4a5d-4d5c-925e-f8ee26efb3c3
type=802-11-wireless

[802-11-wireless]
mode=infrastructure
ssid=<SSID>


[802-11-wireless-security]
auth-alg=open
key-mgmt=wpa-psk
psk=<WPAKEY>

[ipv4]
method=auto

[ipv6]
addr-gen-mode=stable-privacy
method=auto
```

- Place SD card in Raspberry Pi and boot up (takes couple of minutes).

- Access Home Assistant by typing [http://hassio:8123](http://hassio:8123) in the browser.
  - If that doesn't work then wait some more or check local IP for hassio in router page and add :8123 to access Home Assistant.
  - Hassio IP can be fixed in router settings and a port forwarding rule can also be associated to port 8123 with that IP if required.

- First time setup information will need to be obtained:
  - Longitude, Latitude, and Elevation can be obtained from [here](https://www.maps.ie/coordinates.html) against location.
  - Timezone can be confirmed from [here](http://www.timezoneconverter.com/cgi-bin/findzone.tzc) against location.

## Raspberry Pi Setup on Raspbian (without Hassio Supervisor)

### Requirements

- Raspberry Pi (2/3/4)
- SD card (16/32GB)
- Raspbian installation

### Installation Prerequisites

- Open Terminal and check for latest updates on Raspbian and its packages:

```
$ sudo apt-get update
$ sudo apt-get full-upgrade
```

- Install the dependencies that are needed for homeassistant:

```
$ sudo apt-get install python3 python3-venv python3-pip
```

- Setup a Home Assistant Environment
  - Create a user:
  ```
  $ sudo useradd -rm homeassistant
  ```
  - Create a directory:
  ```
  $ cd /srv
  $ sudo mkdir homeassistant
  ```
  - Change owner of directory to user:
  ```
  $ sudo chown homeassistant:homeassistant homeassistant/
  ```
  - Run bash as homeassistant user:
  ```
  $ sudo su -s /bin/bash homeassistant
  ```
  - Switch to homeassistant directory:
  ```
  $ cd srv/homeassistant
  ```
  - Setup a Python virtual environment:
  ```
  $ python3 -m venv homeassistant_venv
  ```
  - Activate to setup environment variables for the virtual environment:
  ```
  $ source /srv/homeassistant/homeassistant_venv/bin/activate
  ```
  - Exit the environment:
  ```
  $ exit
  ```

- For ease of switching to the virtual environment, which is where all testing for configuring the home assistant is run, put the source command in .bashrc of homeassistant user to make it easier.
  - Open bashrc for homeassistant user:
  ```
  $ cd ~
  $ sudo vi .bashrc
  ```
  - Scroll to the bottom of the file, and press I to enter line edit mode
  - Copy the following and paste at the bottom of the file:
  ```
  $ source /srv/homeassistant/homeassistant_venv/bin/activate
  ```
  - When pasted, press ESC to exit edit mode.
  - Press :wq! to save the file.
  - To test the auto load of the environment type the following:
  ```
  $ sudo su -s /bin/bash homeassistant
  ```
  - The following should be observed and be exited after:
  ```
  (homeassistant_venv) homeassistant@raspberrypi:/home/pi $
  ```

### Installation

- Inside the homeassistant virtual environment execute the following commands to install Home Assistant:

```
(homeassistant_venv) homeassistant@raspberrypi:/home/pi $ cd /srv/homeassistant
(homeassistant_venv) homeassistant@raspberrypi:/srv/homeassistant/ $ pip3 install homeassistant
```

- Manually run the program from the virtual environment with the following command however, it can be configured to start when the pi starts up through the systemctl:

```
(homeassistant_venv) homeassistant@raspberrypi:/home/pi $ hass
```

- Set up auto start
  - Begin with creating the service file for this as starting as the pi user:
  ```
  $ sudo su root
  $ cd /etc/systemd/system/
  $ vi home-assistant@pi.service
  ```
  - Cut and paste the following:
  ```
  [Unit]
  Description=Home Assistant
  After=network.target
  
  [Service]
  Type=simple
  User=homeassistant
  ExecStartPre=source /srv/homeassistant/homeassistant_venv/bin/activate
  ExecStart=/srv/homeassistant/homeassistant_venv/bin/hass -c "/home/homeassistant/.homeassistant"
  
  [Install]
  WantedBy=multi-user.target
  ```
  - Save and exit out of editing the file.
  - Exit root to return to the pi user.
  - Restart the systemctl and read the file with the following commands:
  ```
  $ sudo systemctl --system daemon-reload
  $ sudo systemctl enable home-assistant@pi
  
  $ sudo systemctl start home-assistant@pi
  ```
  - By now the service should be able to start with the following command:
  ```
  $ sudo systemctl start home-assistant@pi
  ```
  - The log can be viewed to check if it is starting properly with the command:
  ```
  $ sudo systemctl status home-assistant@pi -l
  ```
  - Or view a scrolling log you can issue the following command:
  ```
  $ sudo journalctl -f -u home-assistant@pi
  ```
  
## Recommended Add-ons Setup

### Configurator - Automations and Configurations YAML Editor

- Click sidebar option "Hass.io".

- Click "ADD-ON STORE".

- Search "Configurator".

- Click "Configurator" and click "Install".

- Once installation completes click "Start".

- For easier configurator access, toggle "Show in sidebar" to on.

- All automations and configurations in the project go in the appropriate YAML file in the configurator.

### Duck DNS - Remote Access

- Click sidebar option "Hass.io".

- Click "ADD-ON STORE".

- Search "Duck DNS".

- Click "Duck DNS" and click "Install".

- Visit DuckDNS.org and create an account by logging in through any of the available account services (Google, Github, Twitter, Persona).

- In the Domains section, type the name of the subdomain you wish to register and click add domain.

- If registration was a success, the subdomain is listed in the Domains section along with current ip being the public IP address of the device you are currently using to access duckdns.org. The IP address will be updated in future by the DuckDNS add-on.

- In the Duck DNS add-on configuration
  - Copy the DuckDNS token (listed at the top of the page where account details are displayed) from duckdns.org and paste into the token option.
  - Update the domains option with the full domain name you registered. E.g., my-domain.duckdns.org.
  - Optionally set "accept_terms": true for a SSL certificate to be issued through Lets Encrypt.
