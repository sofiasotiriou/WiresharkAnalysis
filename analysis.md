## WiresharkAnalysis analysis of ICMP and DNS/HTTP protocols 
#### This analysis is separated in two parts. The first part studies the use of the ICMP protocol, while the second part studies the use of the DNS and HTTP protocols.  

### ICMP protocol 
There were 244 packets detected in ~50 seconds <br>
![Time and No of packets](../WiresharkAnalysis/images/ICMP/1ICMP.png)

The protocols detected in that time are shown in the screenshot below. <br>
![Protocols detected](../WiresharkAnalysis/images/ICMP/2ICMP.png) <br>
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
![ICMP Echo Request Headers](../WiresharkAnalysis/images/ICMP/3ICMP.png)

As is visible from these headers, the devices that communicate through Ethernet are: 
•	Intel_c0:99:55 (source device with MAC address f0:57:a6:c0:99:55) 
•	zte_53:41:c0 (destination device with MAC address 00:4a:77:53:41:c0)
and the IP address of the destination is 192.229.133.221

Since the IPv4 version of the IP protocol is used, the packet has a TTL (and not a hop limit, which is used in the IPv6 version) of 1 and a data length of 64 bytes. <br>
![ICMP Echo Request TTL](../WiresharkAnalysis/images/ICMP/4ICMP.png)  ![ICMP Echo Request Length](../WiresharkAnalysis/images/ICMP/5ICMP.png)

The first ICMP TTL exceeded packet is the next one (the 13th packet to be captured) with the following headers: <br>
![ICMP TTL Exceeded Headers](../WiresharkAnalysis/images/ICMP/6ICMP.png)

Here are all the source IP addresses of the ICMP TTL Exceeded packets: <br>
![ICMP TTL Exceeded sources](../WiresharkAnalysis/images/ICMP/7ICMP.png) <br>
When compared to the IP addresses that are appeared when the tracert command was first deployed, it's seen that the IP addresses above when grouped by three correspond to the adresses on the tracert command. <br>
![ICMP TTL Exceeded tracert](../WiresharkAnalysis/images/ICMP/8ICMP.png)


### DNS and HTTP protocols
The captured traffic contains 118 UDP packets. <br>
![UDP](../WiresharkAnalysis/images/DNSnHTTP/1DNS.png) <br>
And 2272 TCP packets. <br>
![TCP](../WiresharkAnalysis/images/DNSnHTTP/2DNS.png)

The endpoints with which there is a connection on an Ethernet level are 5: <br>
![Ethernet Endpoints](../WiresharkAnalysis/images/DNSnHTTP/3DNS.png) <br>
Using the WiresharkAnalysis OUI search tool it was found that the first device (MAC address: 00:4a:77:53:41:c0) corresponds to a device that was produced by the company ZTE (so a phone, a tablet or a computer), while the last device (MAC address: f0:57:a6:c0:99:55) has the Intel beginning (f0:57:a6) so it is a computer. For the other three MAC addresses no data was found. 

On an IP level, the endpoints differ depending on on the version of the IP protocol (15 endpoints for IPv4-left and 20 for IPv6-right). <br>
![IPv4 Endpoints](../WiresharkAnalysis/images/DNSnHTTP/4DNS.png)
![IPv6 Endpoints](../WiresharkAnalysis/images/DNSnHTTP/5DNS.png) <br>
These endpoints do not correspond with the Ethernet endpoints because they represent different things. The IP endpoints are temporary IP addresses that represent an unintelligible point in the network which is traceable from anywhere. The Ethernet endpoints are MAC addresses which is a permanent marker for a specific device in the local network.

The port that the DNS server uses to receive queries and send responces is the UDP port 53. But the ports the device communicating with the DNS server uses usually vary, depending on the objective of the queries and their responses. 

The objective of a packet (whether it carries a query to or a query responce from the DNS server) is visible from the source/destination IP addresses and ports. When the destination address is that of the DNS server and the destination port is 53, then it is packet directed towards the server, so it is a query (and vice versa). Also, it is stated in the info field of a packet 
![Query markers](../WiresharkAnalysis/images/DNSnHTTP/6DNS.png)
In a similar fashion, when the destination address is that of the computer and the destination port is other that 53 (in this case 49664), the packet contains a query response (and similarly it is written in the info field).
![Query Response markers](../WiresharkAnalysis/images/DNSnHTTP/7DNS.png)

The query and corresponding query response packets are connected through a unique Transaction ID in the info field. Below is an example of said ID in the query packet. The same information can be found in the same places in the query response packet. <br>
![Transaction ID](../WiresharkAnalysis/images/DNSnHTTP/8DNS.png)

There is a flag that defines if the name server responding to the computer's queries is authoritative for that specific domain. More specifically, in the query responce packets there is a field with different flags. One of them is the Authoritative flag, which in this specific scenario is set to 0, meaning that the server is not authoritative for the specific domain that we are working with. 
![Authoritative](../WiresharkAnalysis/images/DNSnHTTP/9DNS.png)

The name of the first website visited (ccslab.aueb.gr) is a regular DNS name and not an alias because the name is type A. The A record corresponds an IP address to a specific host name. If an alias was used, then a CNAME record would have been needed to correspond a domain name (alias) to another domain. As shown below, the query for the A record receives as a responce the IP address of ccslab.aueb.gr (83.212.207.19) directly so there is no alias.  <br>
![Alias](../WiresharkAnalysis/images/DNSnHTTP/10DNS.png)

The first three TCP segments between the computer and the system that hosts ccslab.aueb.gr implement a three-way handshake to establish a connection as seen below: 
![3 Way Handshake](../WiresharkAnalysis/images/DNSnHTTP/11DNS.png)  <br>
The computer (the source of the first segment) in the first segment sends a start-communication request to the ccslab.aueb.gr server (the destination of the first segment). This can be deduced from the [SYN] field in the in the info area of the packet which means that that the flag syn=1 (where syn=synchronize sequence). Then, the server responds to the request with the second segment which sets the flags syn, ack=1 (fields [SYN], [ACK] in the info area). The syn=1 means that the server has accepted the communication and ack=1 means that the server has acknowledged the message it has received. Finally, the third segment of the 3 way handshake only has ack=1 so that the computer can show the server that it has received the second segment and so communication has been established both ways and the transport of data can commence in the info field. 

The TCP ports used in the exchange between the computer and the ccslab.aueb.gr server differ. The server receives and sends segments exclusively through TCP port 80 (the port handling the HTTP protocol), while the computer sends and receives messages through different ports, which are all in the 59720-59735 range, meaning they are dynamic/private ports that are allocated to a computer at the start of a process.

The packets that contain a HTTP GET request are visible and they can be seen all together with the addition of the http.request.method=="GET". They were all sent to the IP address of the server that hosts the website "ccslab.aueb.gr" <br>
![HTTP GET requests](../WiresharkAnalysis/images/DNSnHTTP/1HTTP.png) 

In the first HTTP GET message from the computer to the server, it is visible that this particular IP datagram has not been fragmented. If it had been, then in the details of the headers of the packet there would have been an additional field which for each n fragments would have said [n reassembled segments]. <br>
For reference, this are the fields of the header of the HTTP GET request from the ccslab.aueb.gr server:
![HTTP not fragmented](../WiresharkAnalysis/images/DNSnHTTP/2HTTP.png) <br>
These are the fields of of the answer to the HTTP GET which have been fragmented: <br>
![HTTP fragmented](../WiresharkAnalysis/images/DNSnHTTP/3HTTP.png) <br>
For this part of the analysis, before the capturing the DNS cache was cleared using the ipconfig/flushdns. The captured traffic (packetakia2.pcapng)reflects the visiting of http://ccslab.aueb.gr and https://eclass.aueb.gr through a browser. 

The browser uses the HTTP version 1.1 as every packet at the end of the info area has HTTP/1.1 written. <br>
The connection is persistent, as is the default setting of HTTP 1.1 and no changes have been made during the starting up of the protocol. As is shown below, the connection is "keep-alive", so it is persistent. <br>
![HTTP persistent](../WiresharkAnalysis/images/DNSnHTTP/4HTTP.png)

The message with which the web server responds to the first HTTP GET request is the following: 
![HTTP GET response](../WiresharkAnalysis/images/DNSnHTTP/5HTTP.png) <br>
As is visible above, the server uses the HTTP version 1.1 <br>

The OS used by the web server is Ubuntu: <br>
![Web server OS](../WiresharkAnalysis/images/DNSnHTTP/6HTTP.png)

The size of the file sent by the web server is 29322 bytes and the file type is line based text: 
![HTTP GET response info](../WiresharkAnalysis/images/DNSnHTTP/7HTTP.png)

The first frame that is exchanged between the computer and the server hosting eclass.aueb.gr is the following: 
![First eclass frame](../WiresharkAnalysis/images/DNSnHTTP/8HTTP.png)  <br>
In this frame the computer asks from the DNS server the IPv6 address of eclass.aueb.gr. This is visible from the AAAA in the info packet. The AAAA record matches IPv6 addresses to a specific host name (in this case eclass.aueb.gr). This is the first frame because in order to establish any type of connection/data transfer the computer has to know beforehand the IP address of eclass.aueb.gr so the first thing to be asked for is the request for an IP address (first IPv6 and then IPv4).

The DNS server of eclass.aueb.gr receives reqeusts from UDP port 53 which is the port that handles DNS server communications. 
![DNS Server port](../WiresharkAnalysis/images/DNSnHTTP/9HTTP.png)

The content of the HTTP messages between the computer and the server hosting eclass.aueb.gr are not visible because eclass.aueb.gr uses an alias (as is visible below, in the request for the IPv6 address of eclass.aueb.gr the CNAME record was given) and not a DNS name. <br>
![eclass alias](../WiresharkAnalysis/images/DNSnHTTP/10HTTP.png) <br>
So, the messages are rerouted and when they reach the computer they are encrypted and consequently not readable (by us) by the TSL protocol. 
![TSL encruption](../WiresharkAnalysis/images/DNSnHTTP/11HTTP.png)

The computer and eclass.aueb.gr use version 1.3 (as is seen above) and 1.2 (as seen below)of the TLS protocol.
![Transport Layer Security version](../WiresharkAnalysis/images/DNSnHTTP/12HTTP.png)