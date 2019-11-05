
Master
```
    iptables -I INPUT 5 -p tcp --dport 2376 -j ACCEPT
    iptables -I INPUT 6 -p tcp --dport 2377 -j ACCEPT
    iptables -I INPUT 7 -p tcp --dport 7946 -j ACCEPT
    iptables -I INPUT 8 -p udp --dport 7946 -j ACCEPT
    iptables -I INPUT 9 -p udp --dport 4789 -j ACCEPT
```

workers
```
    iptables -I INPUT 5 -p tcp --dport 2376 -j ACCEPT
    iptables -I INPUT 6 -p tcp --dport 7946 -j ACCEPT
    iptables -I INPUT 7 -p udp --dport 7946 -j ACCEPT
    iptables -I INPUT 8 -p udp --dport 4789 -j ACCEPT
```

services
```
iptables -I INPUT 10 -p tcp --dport 80 -j ACCEPT
```
