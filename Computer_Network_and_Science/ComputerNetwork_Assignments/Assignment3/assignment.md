# Solutions to Computer Network Assignment 3
## I.  Go-Back-N (GBN) Protocol
### (1) Possible Sequence Number Set in the Sender's Window
The receiver is currently expecting the k-th packet, which means all packets with sequence numbers less than k have been successfully acknowledged. The GBN protocol uses a fixed-size continuous sender window with a size of 4. 

Thus, the starting sequence number of the sender's window (base) can range from k to k+3 (if the base exceeded k+3, the window would go beyond the scope of unacknowledged packets, violating the protocol logic). The possible sequence number sets are: [k, k+1, k+2, k+3], [k+1, k+2, k+3, k+4], [k+2, k+3, k+4, k+5], and [k+3, k+4, k+5, k+6], with all sequence numbers falling within the range 1~1024.

### (2) Possible Values of the ACK Field
The ACK field indicates the sequence number of the next packet the receiver expects to receive. GBN uses in-order acknowledgment, so the receiver will not send out-of-order ACKs.

The minimum ACK value is k, which occurs when the receiver has not yet received the k-th packet (only acknowledging packets before k). The maximum ACK value is k+4, which happens when the receiver has successfully received all four packets (k, k+1, k+2, k+3) in the sender's window and now expects k+4. 

Between these two extremes, the receiver may have received 1 to 3 in-order packets, corresponding to ACK values of k+1, k+2, and k+3. Therefore, the possible values of the ACK field are k, k+1, k+2, k+3, and k+4.

## II. SDN OpenFlow Flow Table Configuration
### (1) Flow Table Entries for Switch S1
| Matching Conditions | Action |
|---------------------|--------|
| Input Port=4, Source IP=10.2.0.3 (h3), Destination IP=10.3.0.5 (h5) | Output to Port 1 |
| Input Port=4, Source IP=10.2.0.3 (h3), Destination IP=10.3.0.6 (h6) | Output to Port 1 |
| Input Port=4, Source IP=10.2.0.4 (h4), Destination IP=10.3.0.5 (h5) | Output to Port 1 |
| Input Port=4, Source IP=10.2.0.4 (h4), Destination IP=10.3.0.6 (h6) | Output to Port 1 |
| Input Port=4, Source IP=10.3.0.5 (h5), Destination IP=10.2.0.3 (h3) | Output to Port 4 |
| Input Port=4, Source IP=10.3.0.5 (h5), Destination IP=10.2.0.4 (h4) | Output to Port 4 |
| Input Port=4, Source IP=10.3.0.6 (h6), Destination IP=10.2.0.3 (h3) | Output to Port 4 |
| Input Port=4, Source IP=10.3.0.6 (h6), Destination IP=10.2.0.4 (h4) | Output to Port 4 |
| Input Port=1 or 4, Destination IP=10.1.0.1 (h1) | Output to h1's access port |
| Input Port=1 or 4, Destination IP=10.1.0.2 (h2) | Output to h2's access port |
| Source IP=10.1.0.1 (h1), Destination IP=10.1.0.2 (h2) | Output to h2's access port |
| Source IP=10.1.0.2 (h2), Destination IP=10.1.0.1 (h1) | Output to h1's access port |

### (2) Flow Table Entries for Switch S3 (Firewall Behavior)
| Matching Conditions | Action |
|---------------------|--------|
| Source IP=10.1.0.2 (h2), Destination IP=10.3.0.5 (h5) | Allow Forwarding (Output to the corresponding port) |
| Source IP=10.1.0.2 (h2), Destination IP=10.3.0.6 (h6) | Allow Forwarding (Output to the corresponding port) |
| Source IP=10.2.0.3 (h3), Destination IP=10.3.0.5 (h5) | Allow Forwarding (Output to the corresponding port) |
| Source IP=10.2.0.3 (h3), Destination IP=10.3.0.6 (h6) | Allow Forwarding (Output to the corresponding port) |
| Source IP=10.1.0.1 (h1), Destination IP=10.3.0.5 (h5) | Drop |
| Source IP=10.1.0.1 (h1), Destination IP=10.3.0.6 (h6) | Drop |
| Source IP=10.2.0.4 (h4), Destination IP=10.3.0.5 (h5) | Drop |
| Source IP=10.2.0.4 (h4), Destination IP=10.3.0.6 (h6) | Drop |

## III. Packet Scheduling Algorithms
### (1) Priority Scheduling (Class 1 > Class 2 > Class 3)
Scheduling Rule: Prioritize Class 1 packets, followed by Class 2, and then Class 3. For packets of the same class, transmit them in the order of arrival. One packet is transmitted per time slot.

| Time Slot t | Transmitted Packet | Reason |
|-------------|--------------------|--------|
| 1           | 1                  | Class 2, arrival time 0.35 (the earliest high-priority packet available, no Class 1 packets) |
| 2           | 2                  | Class 2, arrival time 0.85 (the next Class 2 packet in arrival order) |
| 3           | 6                  | Class 1, the only Class 1 packet (highest priority) |
| 4           | 3                  | Class 2, arrival time 1.25 (the earliest remaining Class 2 packet) |
| 5           | 5                  | Class 2, arrival time 1.97 (the next remaining Class 2 packet) |
| 6           | 7                  | Class 2, arrival time 2.93 (the next remaining Class 2 packet) |
| 7           | 10                 | Class 2, arrival time 3.95 (the last remaining Class 2 packet) |
| 8           | 4                  | Class 3, arrival time 1.68 (the earliest Class 3 packet) |
| 9           | 8                  | Class 3, arrival time 3.24 (the next Class 3 packet) |
| 10          | 9                  | Class 3, arrival time 3.55 (the last Class 3 packet) |

### (2) Round-Robin Scheduling (Cycle: Class 1 → Class 2 → Class 3)
Scheduling Rule: Cycle through classes in the order of Class 1 → Class 2 → Class 3. Transmit the earliest arrived packet of the current class if available; skip the class if no packets are queued.

| Time Slot t | Transmitted Packet | Reason |
|-------------|--------------------|--------|
| 1           | 1                  | Cycle to Class 1 (no packets) → Class 2 (has packets, earliest is 1) |
| 2           | 2                  | Cycle to Class 3 (no packets) → Class 1 (no packets) → Class 2 (next is 2) |
| 3           | 4                  | Cycle to Class 3 (has packets, earliest is 4) |
| 4           | 6                  | Cycle to Class 1 (has packets, only 6) |
| 5           | 3                  | Cycle to Class 2 (earliest remaining is 3) |
| 6           | 8                  | Cycle to Class 3 (earliest remaining is 8) |
| 7           | 5                  | Cycle to Class 1 (no packets) → Class 2 (earliest remaining is 5) |
| 8           | 9                  | Cycle to Class 3 (earliest remaining is 9) |
| 9           | 7                  | Cycle to Class 1 (no packets) → Class 2 (earliest remaining is 7) |
| 10          | 10                 | Cycle to Class 3 (no packets) → Class 1 (no packets) → Class 2 (last remaining is 10) |
