# wolforward

Wake-on-LAN when you have more than one target in a network.

Was made for the case where:
- online WOL tool sends WOL packet to my hope IP (unicast)
- modem forwards port 7 and/or 9 to the single host with this service (still unicast)
- this service broadcasts it on my home's subnet


It can also be a standalone CLI wol-sending tool (unicast or local broadcast)



Install
===
Only uses the standard library. 

You probably want to configure this as a service. Aside from convenience of starting at boot,
it's also the easier way to bind to privileged ports (7 and/or 9).

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

