# MTU Calcs

## Protocol overheads

| Protocol   | Bytes |
| ---------- | ----- |
| IP         | 20    |
| TCP        | 20    |
| UDP        |  8    |
| ICMP       |  8    |
| IPSec      | 52    |
| L2TP       | 12    |     
| PPP        | 10    |
| Ethernet   | 14    |

## Common IP MTU

| Network  | MTU  |
|--------- | ---- |
| Internet | 1500 |
| L2TP     | 1450 |

## TCP MSS
IP MTU - 40 bytes


## Pings

Linux requires the size of the ICMP data e.g. to test 1500 byte MTU
```
ping x.x.x.x -M do -s 1472
```

Routers often use the "size" option to be specify the size (bytes) of the IP packet to send (i.e. it does the overhead calcs for you so size may be 1500  in some cases)
