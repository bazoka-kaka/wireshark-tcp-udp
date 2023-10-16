# Wireshark TCP & UDP Jarkom Assignment

<table>
<tbody>
  <thead>
    <tr>
      <th>Name</th>
      <th>NRP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Yehezkiel Wiradhika</td>
      <td>5025201086</td>
    </tr>
  </tbody>
</table>

## Wireshark TCP

### Capturing the packets

- First, we have to download the file from http://gaia.cs.umass.edu/wiresharklabs/alice.txt and retrieve an ASCII copy of Alice in Wonderland. Store this as a .txt file somewhere in our computer.
- Go to http://gaia.cs.umass.edu/wireshark-labs/TCP-wireshark-file1.html.
- We should see something like this
  <img src="assets/webpage.png" />
- We then Browse button in this form to the file on our computer that we just
  created containing Alice in Wonderland. Don’t press the “Upload alice.txt file”
  button yet.
- Now, we start up Wireshark and begin capturing packets
  <img src="assets/start_capture.png" />
- Returning to our browser, press the “Upload alice.txt file” button to upload the
  file to the gaia.cs.umass.edu server. Once the file has been uploaded, a short
  congratulations message will be displayed in your browser window.
  <img src="assets/congratulations.png" />
- Stop Wireshark packet capture

### Questions and Answers

1. What is the IP address and TCP port number used by the client computer (source)
   that is transferring the alice.txt file to gaia.cs.umass.edu?

   First, we have to filter <code>http</code> in our wireshark display filter

   <img src="assets/source_ip.png" />

   Here, we can see that the source IP address of the client computer is <code>192.168.18.17</code> and we know this because the packet containing the source IP is doing a POST request (uploading the file).

   We can see the source port in the details of the TCP packet header window

   <img src="assets/source_port.png" />

   Here, we can see that the source port is <code>50229</code>

2. What is the IP address of gaia.cs.umass.edu? On what port number is it sending
   and receiving TCP segments for this connection?

   <img src="assets/dest_ip.png" />

   Here, we can see that the packet is sending (bc it uses the POST method) the file alice.txt and the destination is gaia.cs.umass.edu. We can see here that the IP of the destination is <code>128.119.245.12</code>. We can see the port when the POST of the file happened in the _TCP header details_

   <img src="assets/dest_port_upload.png" />

   Here, we can see that the destination PORT (port that is receiving the TCP packet for gaia.cs.umass.edu) is <code>port 80</code>, then if we see the TCP details of the response packet

   <img src="assets/src_port_upload.png" />

   We can see that the source PORT (port that is sending the response for gaia.cs.umass.edu) is <code>port 80</code>

3. What is the sequence number of the TCP SYN segment that is used to initiate the
   TCP connection between the client computer and gaia.cs.umass.edu?

   We can filter first with the keyword <code>tcp</code> then search for packets with _SYN_ type and source IP of client (our computer) with destination IP of _gaia.cs.umass.edu_

   <img src="assets/syn_packet.png" />

   We can see in the packet containing the SYN (in the TCP details)

   <img src="assets/syn_raw.png" />

   There (in the highlighted area), you can see the raw TCP sequence number.

4. What is the sequence number of the SYNACK segment sent by gaia.cs.umass.edu
   to the client computer in reply to the SYN? What is it in the segment that
   identifies the segment as a SYNACK segment? What is the value of the
   Acknowledgement field in the SYNACK segment? How did gaia.cs.umass.edu
   determine that value?

   We select the packet that is of type _SYN, ACK_ which has the destination IP of the client and the source IP of _gaia.cs.umass.edu_

   <img src="assets/syn_ack_packet.png" />

   We can find the sequence number and acknowledgement field value of SYNACK segment in the TCP details

   <img src="assets/syn_ack_details.png" />

   in the segment, if we follow the TCP stream, it contains the content of alice.txt

   <img src="assets/syn_ack_content.png" />

   _gaia.cs.umass.edu_ determined the value by using ports and request header from the client that dictates the types and values also other configs in the server&apos;s api

5. What is the sequence number of the TCP segment containing the header of the
   HTTP POST command? How many bytes of data are contained in the payload (data) field of this TCP segment? Did all of the data in the transferred file alice.txt fit into this single segment?

   We can see the sequence number in the packet's tcp details

    <img src="assets/http_post_seq.png" />

   the size (bytes of data) contained in the payload, we can see in the HTTP details response
   <img src="assets/http_post_size.png" />

   All of the data in the transferred file alice.txt doesn't fit in a single segment

   <img src="assets/post_segments.png" />

   rather its disassembled into 109 segments

6. Consider the TCP segment containing the HTTP “POST” as the first segment in
   the data transfer part of the TCP connection.

   - At what time was the first segment (the one containing the HTTP POST) in
     the data-transfer part of the TCP connection sent?
     <img src="assets/tcp_sent_time.png" />
   - At what time was the ACK for this first data-containing segment received?
     <img src="assets/ack_arrival_time.png" />
   - What is the RTT for this first data-containing segment?
     <img src="assets/post_rtt.png">
   - What is the RTT value the second data-carrying TCP segment and its ACK?
     <img src="assets/post_2_rtt.png">
   - What is the EstimatedRTT value (see Section 3.5.3, in the text) after the
     ACK for the second data-carrying segment is received? Assume that in
     making this calculation after the received of the ACK for the second segment, that the initial value of EstimatedRTT is equal to the measured RTT for the
     first segment, and then is computed using the EstimatedRTT equation on
     page 242, and a value of alpha = 0.125.

7. What is the length (header plus payload) of each of the first four data-carrying
   TCP segments?
8. What is the minimum amount of available buffer space advertised to the client by
   gaia.cs.umass.edu among these first four data-carrying TCP segments? Does the
   lack of receiver buffer space ever throttle the sender for these first four data carrying segments?
9. Are there any retransmitted segments in the trace file? What did you check for (in
   the trace) in order to answer this question?
10. How much data does the receiver typically acknowledge in an ACK among the
    first ten data-carrying segments sent from the client to gaia.cs.umass.edu? Can
    you identify cases where the receiver is ACKing every other received segment
    (see Table 3.2 in the text) among these first ten data-carrying segments?
11. What is the throughput (bytes transferred per unit time) for the TCP connection?
    Explain how you calculated this value.
12. Use the Time-Sequence-Graph(Stevens) plotting tool to view the sequence
    number versus time plot of segments being sent from the client to the
    gaia.cs.umass.edu server. Consider the “fleets” of packets sent around t = 0.025, t = 0.053, t = 0.082 and t = 0.1. Comment on whether this looks as if TCP is in its slow start phase, congestion avoidance phase or some other phase. Figure 6 shows a slightly different view of this data.
13. These “fleets” of segments appear to have some periodicity. What can you say
    about the period?
14. Answer each of two questions above for the trace that you have gathered when
    you transferred a file from your computer to gaia.cs.umass.edu
