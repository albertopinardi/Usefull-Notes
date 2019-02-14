Iptables Usefull snippets
===

Portforward TCP/8080 over 192.168.20.113

    iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 8080 -j DNAT --to 192.168.20.113:8080
    iptables -A FORWARD -p tcp -d 192.168.20.113 --dport 8080 -j ACCEPT
