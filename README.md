# Home Assistant Automations and Configurations

## Raspberry Pi Setup

### Requirements

- Raspberry Pi (ZW/2/3/4)
- SD card (16/32GB)

### Installation

- Download [Home Assistant HassOS](https://www.home-assistant.io/hassio/installation/) image.

- Download [Blena Etcher](https://www.balena.io/etcher/) and run it to flash the HassOS image on SD card.

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
