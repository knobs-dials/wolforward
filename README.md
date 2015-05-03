# wolforward

A unicast-to-broadcast relay server for Wake-on-LAN packets.

Also a WOL-packet sender (unicast or local broadcast).


Useful when you have  more than one thing to wake up,
and a host that is always on (to run this service).

The idea is that:
- you use an online WOL tool to send to your home IP (unicast)
- your set your modem up to forward to this service (still unicast)
- this service broadcasts it locally



Install
===
It's an independent script, and the defaults should work fine for most cases.

Currently needs one argument, the pidfile. If you don't care specify /dev/null.

Since it must bind to port 7 and/or 9 to be useful, you will need the rights for that.


Upstart
====
You may well want to run it at boot.  My upstart script looks something like:

        description     "WOL forwarder"
        
        start on (local-filesystems and net-device-up)
        stop on runlevel [!2345]
        
        respawn
        respawn limit 10 5
        
        console log
        
        exec /usr/local/bin/wolforward /dev/null

...the /dev/null because the pidfile argument is currently required, I'll remove that some time.


TODO
===
* get the code py3 ready
* use of pidfile should be an option rather than a requirement