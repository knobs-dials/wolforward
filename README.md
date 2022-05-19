# wolforward

Wake-on-LAN unicast-to-broadcast relay.

Made for a case where:
- online WOL tool sends WOL packet to my home IP, i.e. modem, via unicast
- that modem forwards port 7 and/or 9 to the single host with this service (still unicast)
- this service receives that, and broadcasts it on the subnet it sits on

Useful when you have more than one host you want to wake up in a network,
but your modem (sensibly) won't allow port forwards to the broadcast address.


Use
===
Relay server, as described above

      wolforward -s

Standalone CLI WOL-sending tool - unicast

      wolforward -m 001122334455 -t 192.168.178.222

Standalone CLI WOL-sending tool - broadcast

      wolforward -m 001122334455


Options
===
```
UDP/IP Wake on Lan utility
   -h        print this help, exit
   -v        be more verbose

Use as client:     wolforward -m MAC [options]
   -m MAC    MAC address of target                  - required.  We'll ignore any separator characters in it
   -t IP     IP to send to                          - act as unicast; without this we default to local broadcast
   -p ports  port(s) to send to (comma-separated)   - default: -p 7,9
   -a        amount of packets to send              - default: 40

Use as relay server:  wolforward -s [options]
   -s        run as relay server
   -l IP     listen/bind address                    e.g.  -l 192.168.1.1        default: 0.0.0.0
   -t IP     broadcast target address               e.g.  -t 192.168.1.255      default: halfway clever behaviour of library
   -p ports  port(s) to listen to, comma-separated  e.g.  -p 9  or  -p 7,9,0    default: 7,9
```


Install
===
No dependencies, uses only the python standard library.

You probably want to set it up as a service.
Aside from convenience of starting at boot,
it's also the easier way to allow it to bind to privileged ports (7 and/or 9).

