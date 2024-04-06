# 🔍 Agent-P
*A basic tracking pixel to track email opens.*

## Overview
Spy pixels, also commonly referred to as web beacons, are small artifacts implemented in applications to gather user information, which could include the User-Agent string, IP address, and if an email is opened etc. A basic method to collect this information is through the use of a 1x1 pixel with an embedded URL through a transparent PNG, GIF, JPG.

This project uses Python Flask to serve a static web page with a `pixel.png` included in the `.../image` directory. When a user opens an email with the embedded spy pixel, the User-Agent string, timestamp, and IP Address are logged to the `spy_pixel_logs.txt` file.

### Step 1

Provision a t2.micro Amazon EC2 instance on Ubuntu 20.04.

💡 Make sure to generate or make note of the public/private key pair. A private key will be downloaded by default if you choose to generate one. Also ensure your security group (or cloud firewall) allows HTTP traffic (port 80).

Take note of the Public EC2 DNS domain name.

### Step 2

Log into the ubuntu instance with the private key using SSH. Alternatively, select the EC2 instance and use the online "Connect Profile."

`ssh -i "my_private_key.pem" ubuntu@ec2-publicDNS.amazonaws.com`

Clone this repository

### Step 3

Change directories into the `cd ~/spy-pixel/app`.

Link the the app directory to site-root defined apache configuration `/var/www/html.`: `sudo ln -sT ~/app /var/www/html/app`.

### Step 4

Add the following block below configuration to the `/etc/apache2/sites-enabled/000-default.conf`.

```
WSGIDaemonProcess flaskapp threads=5
WSGIScriptAlias / /var/www/html/flaskapp/flaskapp.wsgi<Directory flaskapp>
    WSGIProcessGroup flaskapp
    WSGIApplicationGroup %{GLOBAL}
    Order deny,allow
    Allow from all
</Directory>
```

### Step 5

Restart the web service: `sudo service apache2 restart`.

Go to `http://ec2-publicDNS.amazonaws.com/image` to see the 1x1 pixel.

### Step 6

Add this 1x1 pixel as img src HTML to a web page or email.

`<img src ="Http://ec2-publicDNS.amazonaws.com/image" height=1 width=1>`



