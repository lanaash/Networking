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
| MPLS       | 4     |
| VLAN       | 4     |
| Ethernet   | 14    |

## Common IP MTU

| Network               | MTU  |
|---------------------- | ---- |
| Internet              | 1500 |
| L2TP over Internet    | 1450 |
| PPPoE                 | 1492 |

Provider networks should operate with a higher MTU and support Jumbo frames so customers can rely upon a minimum 1500 bytes MTU

## TCP MSS
IP MTU - 40 bytes


## Pings

Linux requires the size of the ICMP data e.g. to test 1500 byte MTU
```
ping x.x.x.x -M do -s 1472
```

Routers often use the "size" option to be specify the size (bytes) of the IP packet to send (i.e. it does the overhead calcs for you so size may be 1500  in some cases)
