# wolforward

Wake-on-LAN when you have more than one target in a network, and your router won't allow port forwards to the broadcast address.

Was made for the case where:
- online WOL tool sends WOL packet to my home IP, i.e. modem, via unicast
- modem forwards port 7 and/or 9 to the single host with this service (still unicast)
- this service broadcasts it on the subnet it sits on

(Note: In general this can be done by telling your modem to forward these ports 
to your network's broadcast address, but there are a bunch that are too stupid to understand that)


It can also be a standalone CLI wol-sending tool (unicast or local broadcast)


Options
===
```
Use as client:     wolforward -m MAC [options]
   -m MAC    MAC address of target                  - required, will ignore separator characters
   -t IP     IP to send to                          - unicast to IP; without this we broadcast
   -p ports  port(s) to send to (comma-separated)   - default: -p 7,9
   -a        amount of packets to send              - default: 40

Use as relay server:  wolforward [options] pidfile
   -p ports  port(s) to listen to, comma-separated  e.g.  -p 9  or  -p 7,9,0    default: 7,9
   -l IP     listen/bind address                    e.g.  -l 192.168.1.1        default: 0.0.0.0
   -t IP     broadcast target address               e.g.  -t 192.168.1.255      default: halfway clever
   -f        daemon mode (fork off)
```


Install
===
Only uses the python standard library.

You probably want to set it up as a service.
Aside from convenience of starting at boot,
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

