# WiresharkAnalysis
### A Wireshark analysis of different protocols engaged when visiting specific websites.

This analysis consists of three main parts:  <br>

•	*ICMP protocol* <br>
The traffic captured (packetakia.pcapng file) reflects the command *tracert www.w3schools.com*

•	*DNS and HTTP protocols* <br> 
Before capturing any traffic, for the purposes of the analysis, the DNS cache was cleared using the ipconfig/flushdns command. The captured traffic (packetakia2.pcapng) reflects the visiting of http://ccslab.aueb.gr and https://eclass.aueb.gr through a browser. 

•	*UDP and HTTP *protocols* <br>
 The captured traffic (packetakia3.pcapng) reflects the visiting of http://http.cslab252.cs.aueb.gr/ through a browser. 


It is impportant to note that throughout the three analyses other protocols of different levels of the OSI model are seen in relation to the main protocols analyzed. 