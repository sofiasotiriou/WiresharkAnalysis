### Wireshark analysis of DNS protocol
The captured traffic contains 118 UDP packets. <br>
<img width="806" height="479" alt="Image" src="https://github.com/user-attachments/assets/599c0c48-0457-4f8c-849b-0b6054e39a0a" /><br>
And 2272 TCP packets. <br>
<img width="814" height="481" alt="Image" src="https://github.com/user-attachments/assets/333f3c52-cbfa-4587-9d3f-6a3a8caff5a3" />

The endpoints with which there is a connection on an Ethernet level are 5: <br>
<img width="655" height="224" alt="Image" src="https://github.com/user-attachments/assets/468c0980-5ebf-46ad-9d7b-50b3aefa6634" />
<br>
Using the WiresharkAnalysis OUI search tool it was found that the first device (MAC address: 00:4a:77:53:41:c0) corresponds to a device that was produced by the company ZTE (so a phone, a tablet or a computer), while the last device (MAC address: f0:57:a6:c0:99:55) has the Intel beginning (f0:57:a6) so it is a computer. For the other three MAC addresses no data was found. 

On an IP level, the endpoints differ depending on on the version of the IP protocol (15 endpoints for IPv4-left and 20 for IPv6-right). <br>
<img width="433" height="335" alt="Image" src="https://github.com/user-attachments/assets/0fa7cfab-8d77-461a-bca3-b94b3f0c52c1" />
<img width="477" height="422" alt="Image" src="https://github.com/user-attachments/assets/4ea6bc44-f78a-4ec1-853d-9cdd538b8b65" />
 <br>
These endpoints do not correspond with the Ethernet endpoints because they represent different things. The IP endpoints are temporary IP addresses that represent an unintelligible point in the network which is traceable from anywhere. The Ethernet endpoints are MAC addresses which is a permanent marker for a specific device in the local network.

The port that the DNS server uses to receive queries and send responces is the UDP port 53. But the ports the device communicating with the DNS server uses usually vary, depending on the objective of the queries and their responses. 

The objective of a packet (whether it carries a query to or a query responce from the DNS server) is visible from the source/destination IP addresses and ports. When the destination address is that of the DNS server and the destination port is 53, then it is packet directed towards the server, so it is a query (and vice versa). Also, it is stated in the info field of a packet <br>
<img width="812" height="115" alt="Image" src="https://github.com/user-attachments/assets/6b2cc54c-9f01-4fe4-adc0-0db09f788009" />

In a similar fashion, when the destination address is that of the computer and the destination port is other that 53 (in this case 49664), the packet contains a query response (and similarly it is written in the info field).
<img width="812" height="104" alt="Image" src="https://github.com/user-attachments/assets/9354faa6-8550-4742-afa7-b6b336b70db0" />


The query and corresponding query response packets are connected through a unique Transaction ID in the info field. Below is an example of said ID in the query packet. The same information can be found in the same places in the query response packet. <br>
<img width="812" height="143" alt="Image" src="https://github.com/user-attachments/assets/ec091e40-3300-4194-9873-8939693a5278" />


There is a flag that defines if the name server responding to the computer's queries is authoritative for that specific domain. More specifically, in the query responce packets there is a field with different flags. One of them is the Authoritative flag, which in this specific scenario is set to 0, meaning that the server is not authoritative for the specific domain that we are working with. 
<img width="811" height="273" alt="Image" src="https://github.com/user-attachments/assets/97c010d4-0d45-40d6-863a-bb79b9c37b87" />


The name of the first website visited (ccslab.aueb.gr) is a regular DNS name and not an alias because the name is type A. The A record corresponds an IP address to a specific host name. If an alias was used, then a CNAME record would have been needed to correspond a domain name (alias) to another domain. As shown below, the query for the A record receives as a responce the IP address of ccslab.aueb.gr (83.212.207.19) directly so there is no alias.  <br>
<img width="812" height="400" alt="Image" src="https://github.com/user-attachments/assets/e76b5fcf-28db-46a9-b1fe-c87746404731" />


The first three TCP segments between the computer and the system that hosts ccslab.aueb.gr implement a three-way handshake to establish a connection as seen below: <br>
<img width="812" height="79" alt="Image" src="https://github.com/user-attachments/assets/29eb639c-afc4-4d2e-a864-fe3994f0f459" /><br>
The computer (the source of the first segment) in the first segment sends a start-communication request to the ccslab.aueb.gr server (the destination of the first segment). This can be deduced from the [SYN] field in the in the info area of the packet which means that that the flag syn=1 (where syn=synchronize sequence). Then, the server responds to the request with the second segment which sets the flags syn, ack=1 (fields [SYN], [ACK] in the info area). The syn=1 means that the server has accepted the communication and ack=1 means that the server has acknowledged the message it has received. Finally, the third segment of the 3 way handshake only has ack=1 so that the computer can show the server that it has received the second segment and so communication has been established both ways and the transport of data can commence in the info field. 





### HTTP protocol

The TCP ports used in the exchange between the computer and the ccslab.aueb.gr server differ. The server receives and sends segments exclusively through TCP port 80 (the port handling the HTTP protocol), while the computer sends and receives messages through different ports, which are all in the 59720-59735 range, meaning they are dynamic/private ports that are allocated to a computer at the start of a process.

The packets that contain a HTTP GET request are visible and they can be seen all together with the addition of the http.request.method=="GET". They were all sent to the IP address of the server that hosts the website "ccslab.aueb.gr" <br>
<img width="812" height="379" alt="Image" src="https://github.com/user-attachments/assets/71f7f904-1588-41b7-8cae-8151ff26e9fb" />

In the first HTTP GET message from the computer to the server, it is visible that this particular IP datagram has not been fragmented. If it had been, then in the details of the headers of the packet there would have been an additional field which for each n fragments would have said [n reassembled segments]. <br>
For reference, this are the fields of the header of the HTTP GET request from the ccslab.aueb.gr server:
<img width="811" height="106" alt="Image" src="https://github.com/user-attachments/assets/2aebf286-5ebb-4ff8-9a2b-57b1bb15e039" />

<br>
These are the fields of of the answer to the HTTP GET which have been fragmented: <br>
<img width="811" height="136" alt="Image" src="https://github.com/user-attachments/assets/c2b45115-d841-45a9-bb4e-538fd50b4533" />
 <br>
For this part of the analysis, before the capturing the DNS cache was cleared using the ipconfig/flushdns. The captured traffic (packetakia2.pcapng)reflects the visiting of http://ccslab.aueb.gr and https://eclass.aueb.gr through a browser. 

The browser uses the HTTP version 1.1 as every packet at the end of the info area has HTTP/1.1 written. <br>
The connection is persistent, as is the default setting of HTTP 1.1 and no changes have been made during the starting up of the protocol. As is shown below, the connection is "keep-alive", so it is persistent. <br>
<img width="812" height="115" alt="Image" src="https://github.com/user-attachments/assets/05b20565-eb46-4236-8576-128c640f6266" />



The message with which the web server responds to the first HTTP GET request is the following: 
<img width="811" height="115" alt="Image" src="https://github.com/user-attachments/assets/21039b02-e8a0-4aa5-99c0-34a5720c6419" />

 <br>
As is visible above, the server uses the HTTP version 1.1 <br>

The OS used by the web server is Ubuntu: <br>
<img width="811" height="205" alt="Image" src="https://github.com/user-attachments/assets/9158f500-2bf2-4d01-958a-902b96576d79" />



The size of the file sent by the web server is 29322 bytes and the file type is line based text: 
<img width="812" height="284" alt="Image" src="https://github.com/user-attachments/assets/9d220206-b1d9-4f16-aa8c-0723032eabe4" />

The first frame that is exchanged between the computer and the server hosting eclass.aueb.gr is the following: 
<img width="811" height="119" alt="Image" src="https://github.com/user-attachments/assets/ac465da5-47cb-4135-bcd6-0c4238b03e91" />
<br>
In this frame the computer asks from the DNS server the IPv6 address of eclass.aueb.gr. This is visible from the AAAA in the info packet. The AAAA record matches IPv6 addresses to a specific host name (in this case eclass.aueb.gr). This is the first frame because in order to establish any type of connection/data transfer the computer has to know beforehand the IP address of eclass.aueb.gr so the first thing to be asked for is the request for an IP address (first IPv6 and then IPv4).

The DNS server of eclass.aueb.gr receives reqeusts from UDP port 53 which is the port that handles DNS server communications. 
<img width="812" height="135" alt="Image" src="https://github.com/user-attachments/assets/de5f3b6c-d1c5-46cd-a57a-1038678247ec" />

The content of the HTTP messages between the computer and the server hosting eclass.aueb.gr are not visible because eclass.aueb.gr uses an alias (as is visible below, in the request for the IPv6 address of eclass.aueb.gr the CNAME record was given) and not a DNS name. <br>
<img width="816" height="69" alt="Image" src="https://github.com/user-attachments/assets/4024e424-060b-474a-addf-5313355be73d" />
<br>
So, the messages are rerouted and when they reach the computer they are encrypted and consequently not readable (by us) by the TSL protocol. 
<img width="812" height="171" alt="Image" src="https://github.com/user-attachments/assets/8e8ce1af-3b5f-49eb-bff0-09732509f75f" />

The computer and eclass.aueb.gr use version 1.3 (as is seen above) and 1.2 (as seen below)of the TLS protocol.
<img width="812" height="95" alt="Image" src="https://github.com/user-attachments/assets/fc6395b7-34b7-4dec-9d50-b5d82f3e071f" />