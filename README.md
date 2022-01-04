# blazor-plesk-linux
My humble notes about installin **Blazor server** in a **linux** virtual server with **Plesk**. 
Also PostgreSQL server installation 

Disclaimers:
- I am not a professional programmer
- Some of the test had the Italian interface and my translation could be not correct
- All the text contains languade mistakes a lot....
- The provider I selected is Ionox by 1&1 but other provider could offer similar interfaces

## From the beggining

The virtual server provider I selected offer a cheap virtual linux server. The kind assistant helped me to buy a domain, by the virtual server and install the OS.
After I decided to reinstall the server with a different setting:
- select "Server & Cloud" from the user menu
- select the server you bought
- open the "Action" menu
- select "Reintall Image"

a list of linux distrubution are available, I selected Ubuntu 20.04

- tick the "Plesk Obsidian" from the available software
- start the installation that will take several minutes
- record the root password that will be used by the plesk connection and by the shell login
- record the IP address assigned

## Certificates

The provider offers me a section of "Domains and SSL" were I can get a free SSL certificate. Once generated the certificates have to be dowload locally.
There is a SSL certificate, a file .PFX and intermediate certificate.
It is time to login in Plesk: 
```
https://(server IP address)/login_up.php
```
- select the domain
- select "SSL/TLS Certificates"
- select "Dowload or remove existing ceriticates"
- select "Add SSL/TLS certificate"
- provide a name for your certificate
- at "Private key (\*.key)" select the file "xxxx.key" previously downloaded
- at "Certificate (\*crt)" select the file "xxxx.cer" previously downloaded
- at "CA certificate (\*-ca.crt)" select the file "xxxxINTERMEDIATE.cer" previously downloaded
- select "Upload Certificate"

At hosting settins for XXXXX.com, at "Security" section
- select "SSL/TLS support"
- select Permanent SEO..."
- select at "Certificate" the certificate you have just created/uploaded

When you create a subdomain, assign it the same certificate you assigned to the main domain

## Subdomain

In the provider cloud panel:

.
.
.

In Plesk:
.
.
.


## Installing Dotnet Core

From that moment you shoul connect the shell of the server.
I installed PuTTY in my PC, a well known free SSH client for windows.
- launch PuTTY
- type the virtual server IP inside "Host Name" field
- select OPEN

A terminal window opens
- type "root"
- type the password

You do not need to know many linux commands by heart, just only *ls* (and *ls -l*) to list files and *cd* to change directory.

[Install the .NET SDK or the .NET Runtime on Ubuntu](https://docs.microsoft.com/en-us/dotnet/core/install/linux-ubuntu)
[How to Install Dotnet Core on Ubuntu 20.04](https://tecadmin.net/how-to-install-net-core-on-ubuntu-20-04/)

First of all, enable Microsoft packages repository on your Ubuntu system. 

```
wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb 
sudo dpkg -i packages-microsoft-prod.deb 
```

If you are going to create a application or making changes to existing application, you will required .net core sdk package on your system.

```
sudo apt update 
sudo apt install apt-transport-https 
sudo apt install dotnet-sdk-3.1 
sudo apt install dotnet-sdk-5.0 
```

You can use dotnet command line utility to check installed version of .NET Core on your system. To check dotnet version, type:
```
dotnet --version
```

To deply Blazor in linux, you should follow the direction in the Microsoft document

[Host and deply Blazor Server](https://docs.microsoft.com/en-us/aspnet/core/blazor/host-and-deploy/server?view=aspnetcore-5.0)

But you have to take consider the specific condition of the web server in Plesk.

## Configure Apache and nginx

First of all, you must know that Plesk sports a web server teaming up nginx and Apache.
The client interacts with nginx and nginx interacts with Apache.

[Apache with nginx](https://docs.plesk.com/en-US/obsidian/administrator-guide/web-servers/apache-and-nginx-web-servers-linux/apache-with-nginx.70837/)

Both Apache and nginx have to be configured.
In Plesk there is a specific page for the Apache & nginx Settings, but it is a tricky condition because both Apache and nginx have a complex set of conf files, including each other.

[Apache and Nginx Configuration Files](https://docs.plesk.com/en-US/obsidian/administrator-guide/server-web/server-web-apache-e-nginx-linux/file-di-configurazione-apache-e-nginx.68678/)

The piece of setting you provide will glued inside this network of conf files.

In Plesk:
- select the domains page
- select the "Hosting & DNS" tag
- select "Apache & nginx Settings"
- go to "Additional Apache directives" section
- add to the "Additional directives for HTTPS"


```
ProxyRequests		On
ProxyPreserveHost	On

ProxyPass	 / http://localhost:5000/
ProxyPassReverse / http://localhost:5000/
```

In that case I configure the proxy server to lissen to IP port 5000, the standard port used by Blazor.
If you configure two apps running side by side, assign to the second app another IP port (e.g. 6000): replace 5000 with 6000.
With this only addition, the Blazor app runs. But it is not able to start the SignalR websocket and fall back to the default long polling (communication client/server is done via HTTP).
To open the websocket, the nginx engine must be configured to connect the blazor app.

- go to Additional nginx directives
- add to the "Additional nginx directives"

```
location ~ /
{
	proxy_pass             http://127.0.0.1:5000;
	proxy_read_timeout     60;
	proxy_connect_timeout  60;
	proxy_redirect         off;

	proxy_http_version 1.1;
	proxy_set_header Upgrade $http_upgrade;
	proxy_set_header Connection 'upgrade';
	proxy_set_header Host $host;
	proxy_cache_bypass $http_upgrade;
}
```

Also here, the 5000 is the standard IP port. If you need others app running, use other IP port.

## Publishing the Blazor project


## Create the service




## Install PostgreSQL






