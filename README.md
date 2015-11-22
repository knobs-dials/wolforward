# wolforward

Supports Wake-on-LAN when you have more than one target in a network.

It can 
- be a unicast-to-broadcast relay server for  packets
- be a CLI wol-sending tool (unicast or local broadcast)


The setup this was made for:
- using an online WOL tool to send to your home IP (unicast)
- a modem orwarding port 7 and/or 9 to the single host with this service (still unicast)
- this service broadcasts it locally (on its subnet)



Install
===
It's an independent script relying only on the python standard library. 

You probably want to configure this as a service. Aside from convenience of starting at boot,
it'll also imply the rights to bind to privileged ports (7 and/or 9).

My upstart script looks something like:

        description     "WOL forwarder"
        
        start on (local-filesystems and net-device-up)
        stop on runlevel [!2345]
        
        respawn
        respawn limit 10 5
        
        console log
        
        # Defaults should work fine for most cases.
        exec /usr/local/bin/wolforward /dev/null
        # ...the /dev/null is the pidfile argument, I'll make that optional some time.

