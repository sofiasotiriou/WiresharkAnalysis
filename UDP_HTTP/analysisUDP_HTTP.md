## WiresharkAnalysis analysis of UDP protocol


The capture filter used was: *host 192.168.1.24*
This corresponds to my machine’s IP address (identified using the ipconfig command), allowing capture of all incoming and outgoing traffic related to this host. <br>
<img width="940" height="462" alt="Image" src="https://github.com/user-attachments/assets/ec5f0b53-0dda-44c7-8d01-49479b24058b" />

The display filter used to isolate UDP packets was: *udp* <br>
<img width="940" height="173" alt="Image" src="https://github.com/user-attachments/assets/d1ffb93a-d0dc-4a6e-9f98-9f7c0e351b93" />


The first UDP segment sent from the host appears in frame 5.
<img width="940" height="500" alt="Image" src="https://github.com/user-attachments/assets/a0dec2fc-71f2-45be-9713-893c77ff1fd3" />


The UDP header fields are:

- Source Port (2 bytes)
- Destination Port (2 bytes)
- Length (2 bytes)
- Checksum (2 bytes) <br>
As shown below: 
<img width="509" height="515" alt="Image" src="https://github.com/user-attachments/assets/44c6f76d-9ea2-4423-8e8e-398c9b2baf58" />
<img width="351" height="517" alt="Image" src="https://github.com/user-attachments/assets/a08d883c-10ce-4021-9bfe-0740fbcd7b5e" /> <br>
<img width="450" height="415" alt="Image" src="https://github.com/user-attachments/assets/26c7af31-5f2a-4230-a059-5110a62060f9" />
<img width="540" height="314" alt="Image" src="https://github.com/user-attachments/assets/860ed8f7-d54e-4bf9-9149-602c87fb6f6b" /> <br>


The total size of the UDP header is 8 bytes. <br>
The protocol number in the IP header for UDP is 17. <br>
<img width="825" height="380" alt="Image" src="https://github.com/user-attachments/assets/e3525bf2-f922-40aa-b98e-96cf833e9638" />


The segment size is *62* bytes (where the segment is the payload of the IP packet). The payload size is obtained by subtracting the header length from the total length of the packet. In this case 82-20 = 62 bytes. <br>
The length field of the UDP segment header represents the total length of the segment (header length and payload length). <br>
The maximum length of an IP packet is *65,535* bytes, since the total length field of the IPv4 header is 16 bytes, so the maximum number it can represent is 65,535. The size of the IPv4 header ranges from 20 to 60 bytes. To find the largest possible UDP segment, we assume that the IPv4 header has the minimum possible size (20 bytes), so the segment size can be 65,515 bytes (65,507 payload bytes if the 8 bytes of the UDP header are removed). <br>
The *udp* filter displays the packets transferred to and from the DNS server, therefore the UDP protocol is used for communication with the DNS server. <br>
The IP address of the DNS server is: *192.168.1.1* <br>
<img width="1090" height="203" alt="Image" src="https://github.com/user-attachments/assets/3e7c12f4-a3f5-4209-8657-930c51b3e5fe" />


For a DNS query/response (Type A record), the transport layer ports used are:
-Source Port: *56541*
-Destination Port: *53* <br>
Port 53 is used by the DNS server.
The ports used are shown below in the reqeust as well as the responce from the DNS server:
<img width="1089" height="397" alt="Image" src="https://github.com/user-attachments/assets/bec78d81-5921-4b51-bc16-dcfcc97db572" /> <br>
<img width="1089" height="397" alt="Image" src="https://github.com/user-attachments/assets/bec78d81-5921-4b51-bc16-dcfcc97db572" />


### Wireshark analysis of the HTTP protocol 

The display filter used was: *http* <br>
<img width="1090" height="388" alt="Image" src="https://github.com/user-attachments/assets/fde5ecbf-d3d9-4147-8bc0-e780ae7da617" /> <br>

The browser uses the version 1.1 of the HTTP protocol.  <br>
<img width="1077" height="70" alt="Image" src="https://github.com/user-attachments/assets/f422e256-9b56-47b4-b15c-eb9e4472c282" />

The browser accepts only English language content. <br>
<img width="659" height="377" alt="Image" src="https://github.com/user-attachments/assets/896c35a6-72bc-405a-8944-3dab37084b96" />

The filters used are the ones that only show communications with *port 80* which is the default port for client-server communication and sending HTTP requests and responses. The next two filters only show packets that have the value 1 in the SYN flag of the TCP protocol and 0 in the ACK flag of the TCP protocol so that only the packets that constitute the first step in a three-way handshake are displayed and not those of the second step. 9 requests were made for TCP connections with the source ports changing per request and the number of each port increasing (first request port: 52195 and last 52212). <br>
<img width="1089" height="233" alt="Image" src="https://github.com/user-attachments/assets/1372f434-1d73-46b1-bb5d-37ab58f3910f" /> <br>


The filters used are “http” (which only shows HTTP packets) and ip.src = 192.168.1.24 (which only shows packets sent from the client to the server). The client sent 37 GET requests to the web server. <br>
<img width="1123" height="564" alt="Image" src="https://github.com/user-attachments/assets/14249e75-2b37-4ebe-8d9b-8db0b1851aea" />


The content of the page is sent in the body of the http messages from the server to the client. More specifically, the client sends an http get request for a specific file type (e.g. text/html or png files) to which the server responds with the requested file. 

The sequence numbers of a TCP segment are used to place the pieces of a message in the correct order at the recipient. The ack numbers of a segment are used to confirm the receipt of a specific piece and are the sequence number of the next segment in order. In the example below it is shown that the packet has ack number 1232 <br>
<img width="1090" height="125" alt="Image" src="https://github.com/user-attachments/assets/09b372d7-4c39-44e5-a4e4-04b87334d65a" /> <br>
So, the next packet seq number from the client to the server will be 1232. <br>
<img width="1090" height="115" alt="Image" src="https://github.com/user-attachments/assets/588c3fa1-02a3-4ff8-a8aa-7a037a23517b" />

The client received 19 images (PNG and JPEG/JFIF formats) from the same IP address:
195.251.248.252. <br>
<img width="1089" height="181" alt="Image" src="https://github.com/user-attachments/assets/f9467860-84d1-4137-b839-323d5c71b7ed" /> <br>
<img width="1089" height="184" alt="Image" src="https://github.com/user-attachments/assets/68f0fbdd-9da0-4299-b7ac-1c557e4a8bc9" /> <br>

The first GET request does not include an If-Modified-Since header.
The server response status code is 200 (OK), indicating a successful request. <br>
<img width="1089" height="853" alt="Image" src="https://github.com/user-attachments/assets/2071b858-55dd-4ebb-b406-8d67adb92dca" /> <br>
<img width="1089" height="604" alt="Image" src="https://github.com/user-attachments/assets/d4b16826-a4c7-44d5-8858-57e63f2d67b3" /> <br>

The response to the initial GET request also returns 200 (OK).
According to the Date header, the resource was last modified on 04/06/2024. <br>
<img width="948" height="831" alt="Image" src="https://github.com/user-attachments/assets/2c06295c-3657-4a92-9205-e81be372906d" /> <br>

The first GET request does not include a Cookie header. <br>
<img width="1090" height="679" alt="Image" src="https://github.com/user-attachments/assets/d055ce23-2537-4b18-bcdd-66a2c6864449" /> <br>


A subsequent GET request includes a Cookie header with 20 cookie pairs. <br>
<img width="1020" height="738" alt="Image" src="https://github.com/user-attachments/assets/79174860-a48b-4c10-9497-0db1a7b526f8" />