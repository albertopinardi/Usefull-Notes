Bash Usefull snippets
===========

tcpdump
--------

Capture 2000 packet from a interface ens3 with source ip 10.0.90.86 and write to capture2.pcap file
    
    sudo tcpdump -A -s 0 -i ens3 -l -c 2000 -w capture2.pcap 'src 10.0.90.86'

grep
--------

Find IP

    egrep -o '([0-9]+\.){3}[0-9]+'