# WiresharkAnalysis
####A Wireshark analysis of the protocols engaged when visiting specific websites.

This analysis consists of two main parts: 
•	The first part focuses on the use of the ICMP protocol. The traffic captured (packetakia.pcapng file) reflects the command tracert www.w3schools.com

•	The second part focuses on the DNS and HTTP protocols. Before capturing any traffic, for the purposes pf the analysis the DNS cache was cleared using the ipconfig/flushdns command. The captured traffic (packetakia2.pcapng) reflects the visiting of http://ccslab.aueb.gr and https://eclass.aueb.gr through a browser. 

Throughout the analysis other protocols of different levels of the OSI model are seen in relation to the main protocols analyzed. 