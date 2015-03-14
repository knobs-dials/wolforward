# wolforward

A unicast-to-broadcast relay server for Wake-on-LAN packets
Also a unicast-or-local-broadcast WOL sender.

Useful when you have 
more than one thing to wake up,
and a host that is always on to run this service.

The idea is that:
- you use an online WOL tool to send to your home IP (unicast)
- your modem sends it to this server (still unicast)
- this server broadcasts it locally

TODO: 
- add init and upstart scripts for ease of installation
