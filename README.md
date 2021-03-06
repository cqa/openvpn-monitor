# OpenVPN-Monitor


## Summary

OpenVPN-Monitor is a simple python program to generate html that displays the
status of an OpenVPN server, including all current connections. It uses the
OpenVPN management console. It typically runs on the same host as the OpenVPN
server, however it does not necessarily need to.

[![](https://raw.githubusercontent.com/furlongm/openvpn-monitor/gh-pages/screenshots/openvpn-monitor.png)](https://raw.githubusercontent.com/furlongm/openvpn-monitor/gh-pages/screenshots/openvpn-monitor.png)


## Source

The current source code is available on github:

https://github.com/furlongm/openvpn-monitor


## Quick install with virtualenv/pip/gunicorn


```shell
mkdir /srv/openvpn-monitor
cd /srv/openvpn-monitor
virtualenv .
. bin/activate
pip install openvpn-monitor gunicorn
gunicorn openvpn-monitor -b 0.0.0.0:80

```


## Installation

### Install dependencies and configure apache

#### Debian / Ubuntu

```shell
apt-get -y install python-geoip python-ipaddr python-humanize python-bottle apache2 libapache2-mod-wsgi git wget
a2enmod wsgi
echo "WSGIScriptAlias /openvpn-monitor /var/www/html/openvpn-monitor/openvpn-monitor.py" > /etc/apache2/conf-available/openvpn-monitor.conf
a2enconf openvpn-monitor
systemctl restart apache2
```

#### CentOS

```shell
yum install -y epel-release
yum makecache
yum install -y python-GeoIP python-ipaddr python-humanize python-bottle httpd mod_wsgi git wget
echo "WSGIScriptAlias /openvpn-monitor /var/www/html/openvpn-monitor/openvpn-monitor.py" > /etc/httpd/conf.d/openvpn-monitor.conf
systemctl restart apache2
```


### Checkout OpenVPN-Monitor

```shell
cd /var/www/html
git clone https://github.com/furlongm/openvpn-monitor.git
```


### Configure OpenVPN

Add the following line to your OpenVPN server configuration to run the
management console on 127.0.0.1 port 5555:

```
management 127.0.0.1 5555
```

Refer to the OpenVPN documentation for further information on how to secure
access to the management interface.


### Download the GeoLite City database

```shell
cd /usr/share/GeoIP/
wget http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz
gunzip GeoLiteCity.dat.gz
mv GeoLiteCity.dat GeoIPCity.dat
```


### Configure OpenVPN-Monitor

The example configuration file `/var/www/html/openvpn-monitor/openvpn-monitor.conf`
should give some indication of how to set site name, add a logo, etc. You can
also set a default location (latitude and longitude) for the embedded maps.
If not set, the default location is Melbourne, Australia.

Edit `/var/www/html/openvpn-monitor/openvpn-monitor.conf` to match your site.

You should now be able to navigate to `http://myipaddress/openvpn-monitor`


### Debugging

OpenVPN-Monitor can be run from the command line in order to test if the html
generates correctly:

```shell
cd /var/www/html/openvpn-monitor
python openvpn-monitor.py
```


## License

OpenVPN-Monitor is licensed under the GPLv3, a copy of which can be found in
the COPYING file.


## Acknowledgements

Flags are created by Matthias Slovig (flags@slovig.de) and are licensed under
Creative Commons License Deed Attribution-ShareAlike 3.0 Unported
(CC BY-SA 3.0). See http://flags.blogpotato.de/ for more details.
