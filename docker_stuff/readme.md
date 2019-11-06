
Master
```
    iptables -I INPUT -p tcp --dport 2376 -j ACCEPT
    iptables -I INPUT -p tcp --dport 2377 -j ACCEPT
    iptables -I INPUT -p tcp --dport 7946 -j ACCEPT
    iptables -I INPUT -p udp --dport 7946 -j ACCEPT
    iptables -I INPUT -p udp --dport 4789 -j ACCEPT
```

workers
```
    iptables -I INPUT -p tcp --dport 2376 -j ACCEPT
    iptables -I INPUT -p tcp --dport 7946 -j ACCEPT
    iptables -I INPUT -p udp --dport 7946 -j ACCEPT
    iptables -I INPUT -p udp --dport 4789 -j ACCEPT
```

services
```
iptables -I INPUT 10 -p tcp --dport 80 -j ACCEPT
```

elk stack swarm
https://github.com/mattjtodd/docker-swarm-elk
