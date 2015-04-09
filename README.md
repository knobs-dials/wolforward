# wolforward

A unicast-to-broadcast relay server for Wake-on-LAN packets

Also a unicast-or-local-broadcast WOL sender.


Useful when you have  more than one thing to wake up,
and a host that is always on (to run this service).

The idea is that:
- you use an online WOL tool to send to your home IP (unicast)
- your set your modem up to forward to this service (still unicast)
- this server broadcasts it locally


Upstart
===
My upstart script looks something like:

        description     "WOL forwarder"
        
        start on (local-filesystems and net-device-up)
        stop on runlevel [!2345]
        
        respawn
        respawn limit 10 5
        
        console log
        
        exec /usr/local/bin/wolforward /dev/null

...the /dev/null because the pidfile argument is currently required, I'll remove that some time.