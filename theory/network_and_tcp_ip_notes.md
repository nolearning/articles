# Network And TCP/IP Notes

## TCP

```
+-------------------------------------------------------+
|     client           network            server        |
+-----------------+                +--------------------|
|    (connect)    | ---- SYN ----> |                    |
|                 | <-- SYN,ACK -- |     (accepted)     |
|   (connected)   | ---- ACK ----> |                    |
\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/

when client sends...
\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/
|                 |                |                    |
|     (send)      | ---- data ---> |                    |
|                 | <---- ACK ---- |  (data received)   |
\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/

when server sends...
\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/
|                 |                |                    |
|                 | <--- data ---- |       (send)       |
| (data received) | ---- ACK ----> |                    |
\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/
```
...and so on, til the connection is shut down or reset

Most TCP/IP stacks try to reduce the number of naked ACKs without unduly risking retransmission or a connection reset. So a conversation like this one is quite possible:
```
\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/
|                 |                |                    |
|                 | <--- data ---- |       (send)       |
| (data received) |                |                    |
|     (send)      | -- data,ACK -> |                    |
|                 |                |  (data received)   |
|                 | <- data,ACK -- |       (send)       |
| (data received) |                |                    |
|  (wait a bit)   | <--- data ---- |       (send)       |
| (data received) |                |                    |
|     (send)      | -- data,ACK -> |                    |
|                 |                |  (data received)   |
|     (send)      | ---- data ---> |   (wait a bit)     |
|                 |                |  (data received)   |
|                 | <- data,ACK -- |       (send)       |
| (data received) |                |                    |
|  (wait a bit)   |   (dead air)   |                    |
|                 | ---- ACK ----> |                    |
\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/\_/
```
### Three-Way Handshake
  * SYN - (Synchronize) Initiates a connection
  * FIN - (Final) Cleanly terminates a connection
  * ACK - Acknowledges received data
  
#### Why Three-Way Handshake
>   TCP 需要 seq 序列号来做可靠重传或接收，而避免连接复用时无法分辨出 seq 是延迟或者是旧链接的 seq，因此需要三次握手来约定确定双方的 ISN（初始 seq 序列号）。
>A three way handshake is necessary because sequence numbers are not tied to a global clock in the network, and TCPs may have different mechanisms for picking the ISN's. The receiver of the first SYN has no way of knowing whether the segment was an old delayed one or not, unless it remembers the last sequence number used on the connection (which is not always possible), and so it must ask the sender to verify this SYN. The three way handshake and the advantages of a clock-driven scheme are discussed in [3].

> Each state machine setup is completed when the SYN is acknowledged with an ACK.
>
> As an optimization, the server's ACK to client's first SYN and it's own SYN are collapsed into the same packet, which is termed SYN-ACK. The client's ACK to the server's SYN then happens to look like the third packet, which is termed as a "three way hand shake".
>
> It's NOT really THREE packets, but FOUR, optimized to THREE.

```
Alice ---> Bob    SYNchronize with my Initial Sequence Number of X
Alice <--- Bob    I received your syn, I ACKnowledge that I am ready for [X+1]
Alice <--- Bob    SYNchronize with my Initial Sequence Number of Y
Alice ---> Bob    I received your syn, I ACKnowledge that I am ready for [Y+1]

Bob <--- Alice         SYN
Bob ---> Alice     SYN ACK 
Bob <--- Alice     ACK     

```
  
### Sequence Numbers
  1. Each TCP sequence starts with random number, which is Initial Sequence Number (ISN)
  2. It is increased by 1 for each byte transmitted
  3. SYN and FIN flags count as 1 Byte („Phantom Byte“)
  4. An ACK segment, if carrying no data, consumes no sequence number.

>A fundamental notion in the design is that every octet of data sent over a TCP connection has a sequence number.  Since every octet is sequenced, each of them can be acknowledged.  The acknowledgment mechanism employed is cumulative so that an acknowledgment of sequence number X indicates that all octets up to but not including X have been received.

#### Unique ISN
> When new connections are created, an initial sequence number (ISN) generator is employed which selects a new 32 bit ISN.  The generator is bound to a (possibly fictitious) 32 bit clock whose low order bit is incremented roughly every 4 microseconds.  Thus, the ISN cycles approximately every 4.55 hours. Since we assume that segments will stay in the network no more than the Maximum Segment Lifetime (MSL) and that the MSL is less than 4.55 hours we can reasonably assume that ISN's will be unique.

### TCP Retransmissions
* Ways to trigger a retransmission
  - By time out
  - By Triple Duplicate ACK
  - By Selective Acknowledgement(SACK)

## References
* [Transmission Control Protocol](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)
* [Understanding TCP Sequence and Acknowledgment Numbers](http://packetlife.net/blog/2010/jun/7/understanding-tcp-sequence-acknowledgment-numbers/)
* [TCP Sequence & Acknowledgement Numbers](http://www.firewall.cx/networking-topics/protocols/tcp/134-tcp-seq-ack-numbers.html)
* [TCP Analysis - First Steps](https://sharkfestus.wireshark.org/assets/presentations/B5%20-%20TCP%20Analysis%20-%20First%20Steps.pdf)
* [Internet Transport Layer](http://www-sop.inria.fr/members/Vincenzo.Mancuso/ReteInternet/06_tcp_part1.pdf)

* [Does TCP send a SYN/ACK on every packet or only on the first connection?](https://stackoverflow.com/questions/3604485/does-tcp-send-a-syn-ack-on-every-packet-or-only-on-the-first-connection)
* [Why the Sequence number in an ACK packet is incremented?](https://serverfault.com/questions/904756/why-the-sequence-number-in-an-ack-packet-is-incremented)
* [Why does an pure ACK increment the sequence number?](https://networkengineering.stackexchange.com/questions/48775/why-does-an-pure-ack-increment-the-sequence-number)
* [Why do we need a 3-way handshake? Why not just 2-way?](https://networkengineering.stackexchange.com/questions/24068/why-do-we-need-a-3-way-handshake-why-not-just-2-way)

* [In TCP, the client sends a SYN then the server responds with an ACK of the SYN. But then the client goes back and sends another ACK before sending data. What's the point of the second ACK?](https://www.quora.com/In-TCP-the-client-sends-a-SYN-then-the-server-responds-with-an-ACK-of-the-SYN-But-then-the-client-goes-back-and-sends-another-ACK-before-sending-data-Whats-the-point-of-the-second-ACK/answer/Diwakar-Tundlam-1)

* [TCP 为什么是三次握手，而不是两次或四次？](https://www.zhihu.com/question/24853633)
