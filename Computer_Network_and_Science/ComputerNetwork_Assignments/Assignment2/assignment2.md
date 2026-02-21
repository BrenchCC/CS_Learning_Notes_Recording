# Answer
## 1. Differences Between UDP and TCP
- **Connection Type**: TCP is a connection-oriented protocol. It requires a three-way handshake to establish a connection before communication and a four-way handshake to terminate the connection after communication. UDP is a connectionless protocol, allowing communicating parties to send data directly without prior connection establishment.
- **Reliability**: TCP provides reliable data delivery. It ensures data is transmitted without loss, errors, and in order through mechanisms such as acknowledgment, retransmission, flow control, and congestion control. UDP does not guarantee reliable delivery; it cannot ensure data order or integrity, leading to potential packet loss or out-of-sequence delivery.
- **Data Transmission Mode**: TCP is a byte-stream-oriented protocol. It treats application-layer data as a continuous byte stream and transmits it in segments. UDP is a message-oriented protocol. It directly sends messages from the application layer as data units without segmentation (unless exceeding the MTU).
- **Header Overhead**: TCP has more header fields (e.g., sequence number, acknowledgment number, window size), resulting in higher overhead (typically 20 bytes, up to 40 bytes with options). UDP headers only include source port, destination port, length, and checksum, with fixed low overhead (8 bytes).
- **Application Scenarios**: TCP is suitable for scenarios requiring high reliability, such as file transfer (FTP), web browsing (HTTP/HTTPS), and email (SMTP). UDP is ideal for real-time applications tolerating minor packet loss, such as voice calls (VoIP), video streaming (RTSP), and DNS queries.

---

## 2. Subnet Mask Related Questions
- **Definition of Subnet Mask**: A subnet mask is a 32-bit binary number used to distinguish the network portion and host portion of an IP address. When ANDed with an IP address, the result is the corresponding network address. Bits set to "1" represent the network portion, and bits set to "0" represent the host portion.
- **Subnet Mask for 192.16.3.65/23**: The "/23" indicates 23 network bits. Since IPv4 addresses are 32 bits long, the host portion is 32-23=9 bits. The 32-bit binary form of the subnet mask is 11111111.11111111.11111110.00000000, which converts to the decimal 255.255.254.0. Thus, the subnet mask for 192.16.3.65/23 is 255.255.254.0.

---

## 3. Minimum File Distribution Time Calculation
First, unify units: \(F=30\) Gbit \(=30\times1024\) Mbit \(=30720\) Mbit; \(u=700\) Kbps \(=0.7\) Mbps.

### (1) Client-Server Distribution Mode
In this mode, the file is only uploaded by the server. The minimum distribution time is the maximum of two values: the time for the server to complete uploading the file, and the time for the slowest peer to finish downloading. The formula is \(T_{cs}=\max\left(\frac{F}{u_s},\frac{F}{d_i\times N}\right)\) (all peers have the same download rate \(d_i=2\) Mbps).
- When \(N=10\):
  - Server upload time: \(\frac{F}{u_s}=\frac{30720}{30}=1024\) seconds;
  - Peer download time: \(\frac{F}{d_i\times N}=\frac{30720}{2\times10}=1536\) seconds;
  - Minimum distribution time \(T_{cs}=\max(1024,1536)=1536\) seconds.
- When \(N=1000\):
  - Server upload time: \(\frac{F}{u_s}=\frac{30720}{30}=1024\) seconds;
  - Peer download time: \(\frac{F}{d_i\times N}=\frac{30720}{2\times1000}=15.36\) seconds;
  - Minimum distribution time \(T_{cs}=\max(1024,15.36)=1024\) seconds.

### (2) P2P Distribution Mode
In P2P mode, both the server and peers can upload files. The minimum distribution time is the maximum of three values: server upload time, total download demand time of all peers, and time corresponding to total upload capacity. The formula is \(T_{p2p}=\max\left(\frac{F}{u_s},\frac{F}{d_i\times N},\frac{F}{u_s + N\times u}\right)\).
- When \(N=10\):
  - Server upload time: \(\frac{F}{u_s}=\frac{30720}{30}=1024\) seconds;
  - Total peer download demand time: \(\frac{F}{d_i\times N}=\frac{30720}{2\times10}=1536\) seconds;
  - Time for total upload capacity: \(\frac{F}{u_s + N\times u}=\frac{30720}{30 + 10\times0.7}\approx830.27\) seconds;
  - Minimum distribution time \(T_{p2p}=\max(1024,1536,830.27)=1536\) seconds.
- When \(N=1000\):
  - Server upload time: \(\frac{F}{u_s}=\frac{30720}{30}=1024\) seconds;
  - Total peer download demand time: \(\frac{F}{d_i\times N}=\frac{30720}{2\times1000}=15.36\) seconds;
  - Time for total upload capacity: \(\frac{F}{u_s + N\times u}=\frac{30720}{30 + 1000\times0.7}\approx42.08\) seconds;
  - Minimum distribution time \(T_{p2p}=\max(1024,15.36,42.08)=1024\) seconds.

---

## 4. Go-back-N Protocol Related Questions
### (1) Expected Sequence Number at the Receiver
In the Go-back-N protocol, the receiver acknowledges each correctly received packet in order. The acknowledgment number indicates the sequence number of the next expected packet. The receiver has received packets 0, 1, 2, and 3 in order, so the next expected sequence number is 4. Even if all ACKs are lost, the receiver’s expected sequence number remains 4.

### (2) Receiver’s Operation After Receiving Retransmitted Packet 0
The receiver has already received packet 0 in order and expects packet 4. The retransmitted packet 0 is obsolete. According to Go-back-N rules, the receiver discards the obsolete packet 0 and does not send a new ACK (since the original ACK for packet 0 was already sent but lost; retransmitting the ACK might cause misunderstandings by the sender).

---

## 5. TCP Congestion Control Related Questions
### (1) Time for cwnd to Increase from 10 MSS to 22 MSS
TCP congestion control does not use slow start. The cwnd increases by 1 MSS for each batch of ACKs received (i.e., per RTT). To increase from 10 MSS to 22 MSS, the required increment is \(22 - 10 = 12\) MSS. Since each increment takes 1 RTT, the total time is \(12\times RTT\).

### (2) Average Throughput While cwnd Increases from 10 MSS to 22 MSS
Throughput refers to the amount of data transmitted per unit time. During this period, the cwnd sizes per RTT are 10 MSS, 11 MSS, ..., 22 MSS (12 RTTs total).
- Total data transmitted: Sum of the arithmetic sequence \(10 + 11 + ... + 22\). Using the formula \(S_n=\frac{n(a_1 + a_n)}{2}\) (where \(n=12\), \(a_1=10\), \(a_n=22\)), \(S=\frac{12\times(10 + 22)}{2}=192\) MSS.
- Total time: \(12\times RTT\).
- Average throughput: \(\frac{Total~Data}{Total~Time}=\frac{192}{12\times RTT}=\frac{16}{RTT}\) (units: MSS/RTT).

---

## 6. IP Subnetting and Address Allocation
The network address is 201.16.18.0/23 with a subnet mask of 255.255.254.0. It has 9 host bits, supporting \(2^9 - 2 = 510\) usable hosts (excluding network and broadcast addresses). We need to divide it into 5 subnets for 233, 120, 50, 30, and 14 hosts, following the "largest-first" principle to avoid address waste.

### Step 1: Determine Host Bits and Subnet Masks for Each Subnet
- 233 hosts: \(2^h - 2 \geq 233\) → \(h\geq8\) (254 usable hosts). Subnet mask: 255.255.254.128 (/24).
- 120 hosts: \(2^h - 2 \geq 120\) → \(h\geq7\) (126 usable hosts). Subnet mask: 255.255.254.192 (/25).
- 50 hosts: \(2^h - 2 \geq 50\) → \(h\geq6\) (62 usable hosts). Subnet mask: 255.255.254.224 (/26).
- 30 hosts: \(2^h - 2 \geq 30\) → \(h\geq5\) (30 usable hosts). Subnet mask: 255.255.254.240 (/27).
- 14 hosts: \(2^h - 2 \geq 14\) → \(h\geq4\) (14 usable hosts). Subnet mask: 255.255.254.248 (/28).

### Step 2: Allocate Subnet Addresses
The original network range is 201.16.18.0 - 201.16.19.255 (512 addresses total).
- **Subnet 1 (233 hosts)**: /24, range 201.16.18.0 - 201.16.18.255. Network address: 201.16.18.0; Broadcast address: 201.16.18.255; Usable hosts: 201.16.18.1 - 201.16.18.254.
- **Subnet 2 (120 hosts)**: /25, range 201.16.19.0 - 201.16.19.127. Network address: 201.16.19.0; Broadcast address: 201.16.19.127; Usable hosts: 201.16.19.1 - 201.16.19.126.
- **Subnet 3 (50 hosts)**: /26, range 201.16.19.128 - 201.16.19.191. Network address: 201.16.19.128; Broadcast address: 201.16.19.191; Usable hosts: 201.16.19.129 - 201.16.19.190.
- **Subnet 4 (30 hosts)**: /27, range 201.16.19.192 - 201.16.19.223. Network address: 201.16.19.192; Broadcast address: 201.16.19.223; Usable hosts: 201.16.19.193 - 201.16.19.222.
- **Subnet 5 (14 hosts)**: /28, range 201.16.19.224 - 201.16.19.239. Network address: 201.16.19.224; Broadcast address: 201.16.19.239; Usable hosts: 201.16.19.225 - 201.16.19.238.

### Final Address Allocation Table
| Subnet No. | Required Hosts | Subnet Mask | Network Address | Broadcast Address | Usable Host Address Range |
| ---------- | -------------- | ----------- | --------------- | ----------------- | ------------------------- |
| 1          | 233            | 255.255.254.128 (/24) | 201.16.18.0 | 201.16.18.255 | 201.16.18.1 - 201.16.18.254 |
| 2          | 120            | 255.255.254.192 (/25) | 201.16.19.0 | 201.16.19.127 | 201.16.19.1 - 201.16.19.126 |
| 3          | 50             | 255.255.254.224 (/26) | 201.16.19.128 | 201.16.19.191 | 201.16.19.129 - 201.16.19.190 |
| 4          | 30             | 255.255.254.240 (/27) | 201.16.19.192 | 201.16.19.223 | 201.16.19.193 - 201.16.19.222 |
| 5          | 14             | 255.255.254.248 (/28) | 201.16.19.224 | 201.16.19.239 | 201.16.19.225 - 201.16.19.238 |

(Remaining addresses: 201.16.19.240 - 201.16.19.255 as backup.)
