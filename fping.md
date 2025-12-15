## Packet loss hunting across the network

Put your test IPs in ips.txt
```
for i in `cat ips.txt`; do fping -p20 -r0 -q -c50 $i; done
```
