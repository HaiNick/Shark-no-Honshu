# tcpdump packet capture because network debugging.

## basic capture
```bash
sudo tcpdump -i eth0                     # capture on interface
sudo tcpdump port 80                     # specific port
sudo tcpdump host 192.168.1.100         # specific host
sudo tcpdump -A port 110                # ASCII output for port 110
```

## protocol filters
```bash
sudo tcpdump icmp                        # ICMP traffic only
sudo tcpdump tcp                         # TCP traffic only
sudo tcpdump udp                         # UDP traffic only
sudo tcpdump ip proto \\icmp -i tun0     # ICMP on tunnel interface
```

## advanced filters
```bash
sudo tcpdump src 192.168.1.100          # source IP
sudo tcpdump dst 192.168.1.100          # destination IP
sudo tcpdump net 192.168.1.0/24         # network range
sudo tcpdump portrange 80-443           # port range
```

## output options
```bash
sudo tcpdump -n                          # no DNS resolution
sudo tcpdump -v                          # verbose output
sudo tcpdump -X                          # hex and ASCII dump
sudo tcpdump -c 100                      # capture 100 packets then stop
sudo tcpdump -w capture.pcap             # write to file
```

## analysis
```bash
tcpdump -r capture.pcap                  # read from file
tcpdump -r capture.pcap port 80          # filter saved capture
```

## notes
[!] requires root/sudo privileges
[!] capturing on busy interfaces can generate massive output
[!] use filters to focus on relevant traffic