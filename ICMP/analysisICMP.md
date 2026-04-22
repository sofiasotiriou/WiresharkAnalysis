## WiresharkAnalysis analysis of ICMP protocol

There were 244 packets detected in ~50 seconds <br>
<img width="811" height="209" alt="Image" src="https://github.com/user-attachments/assets/90a7e4f6-1075-4d82-9356-d3a70f4c51ee" />
<br>
The protocols detected in that time are shown in the screenshot below. <br>
<img width="752" height="406" alt="Image" src="https://github.com/user-attachments/assets/bca962ff-9cea-4770-94c5-1dd5bcbbb9eb" />
<br>
According to the screenshot, the protocols detected according to the OSI Model Layer they belong to are as follows: <br>
•	Data Link layer: Ethernet <br>
•	Network layer: IPv6, IPv4, ICMPv6, ICMP, ARP <br>
•	Transport layer: UDP, TCP <br>
•	Presentation layer: TLS <br>
•	Application layer: DNS, mDNS, SSTP, NBNS <br>

Each Application layer protocol employs a different Transport layer protocol. They correspond as follows: <br>
•	mDNS and DNS use UDP when either IPv4 or IPv6 is used. <br>
•	SSDP and NBNS use UDP when IPv4 is used. When IPv6 is used they don't exist. 

The filters "icmp" and "icmp6" (to display the ICMP protocol for both IPv4 and IPv6) are employed to isolate the ICMP packets. <br>

The IP packet that contains the first ICMP Echo Request is the 12th packet to be captured with the following headers: 
<img width="811" height="86" alt="Image" src="https://github.com/user-attachments/assets/be46d013-6614-4e2e-890e-118001e73ae5" />
<br>

As is visible from these headers, the devices that communicate through Ethernet are: 
•	Intel_c0:99:55 (source device with MAC address f0:57:a6:c0:99:55) 
•	zte_53:41:c0 (destination device with MAC address 00:4a:77:53:41:c0)
and the IP address of the destination is 192.229.133.221

Since the IPv4 version of the IP protocol is used, the packet has a TTL (and not a hop limit, which is used in the IPv6 version) of 1 and a data length of 64 bytes. <br>
<img width="352" height="107" alt="Image" src="https://github.com/user-attachments/assets/06873d94-9845-4bd5-bbc3-572eabdaa27f" />
<img width="315" height="203" alt="Image" src="https://github.com/user-attachments/assets/5ce2ed83-a91d-4c87-9d4f-44bdbbbe5852" />
<br>

The first ICMP TTL exceeded packet is the next one (the 13th packet to be captured) with the following headers: <br>
<img width="600" height="300" alt="Image" src="https://github.com/user-attachments/assets/a24d04d1-16b3-40c2-af96-032bdee32a25" />
<br>

Here are all the source IP addresses of the ICMP TTL Exceeded packets: <br>

<img width="601" height="336" alt="Image" src="https://github.com/user-attachments/assets/46f0ee13-0a04-45f1-951e-694bf54e4d4b" />
<br>

When compared to the IP addresses that are appeared when the tracert command was first deployed, it's seen that the IP addresses above when grouped by three correspond to the adresses on the tracert command. <br>

<img width="751" height="324" alt="Image" src="https://github.com/user-attachments/assets/fe58ca80-1583-486f-838c-463e3df1d9dc" />
