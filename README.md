# blazor-plesk-linux
My humble notes about installin Blazor server in a linux virtual server with Plesk

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
'''
https://(IP address)/login_up.php
'''
