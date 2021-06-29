# wolforward

Wake-on-LAN unicast-to-broadcast relay.

Useful when you have more than one host you want to wake up in a network,
but your modem won't allow port forwards to the broadcast address (which is sensible).

Was made for the case where:
- online WOL tool sends WOL packet to my home IP, i.e. modem, via unicast
- modem forwards port 7 and/or 9 to the single host with this service (still unicast)
- this service receives that, and broadcasts it on the subnet it sits on


Can also be a standalone CLI WOL-sending tool (unicast or local broadcast)


Options
===
```

Use as relay server:  wolforward [options] pidfile
   -l IP     listen/bind address                    e.g.  -l 192.168.1.1        default: 0.0.0.0
   -t IP     broadcast target address               e.g.  -t 192.168.1.255      default: halfway clever
   -f        daemon mode (fork off)
   -p ports  port(s) to listen to, comma-separated  e.g.  -p 9  or  -p 7,9,0    default: 7,9

Use as client:     wolforward -m MAC [options]
   -m MAC    MAC address of target                  - required. Will ignore separator characters
   -t IP     IP to send to                          - default is local broadcast. Specify this for unicast elsewhere.
   -a        amount of packets to send              - default: 40
   -p ports  port(s) to send to (comma-separated)   - default: -p 7,9
```


Install
===
Only uses the python standard library.

You probably want to set it up as a service.
Aside from convenience of starting at boot,
it's also the easier way to allow it to bind to privileged ports (7 and/or 9).

