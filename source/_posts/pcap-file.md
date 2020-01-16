---
title: PCAP File Structure Analysis
date: 2020-01-14 20:51:30
tags: [wireshark, tools]
---



#### PCAP File Structure Analysis  
This analysis is based on wireshark source code. 

The general structure of pcap file is show in the following: 

| pcap file header | packet header 1 | packet data 1 | packet header 2 | packet data 2 | ... packet header n | ... packet data |
| ---------------- | --------------- | ------------- | --------------- | ------------- | ------------------- | --------------- |
| 24 bytes         | 16 bytes        | ..            | 16 bytes        | ...           | 16 bytes            | ...             |



 A pcap file includes a pcap file header field and some records. record field includes record header and packet data.
 The detail about file header and record are introduced as following.
 
#####  PCAP File Header 

```c
struct pcap_hdr {
  guint32 magic  /* magic */
  guint16 version_major;  /* major version number */
  guint16 version_minor;  /* minor version number */
  gint32  thiszone; /* GMT to local correction */
  guint32 sigfigs;  /* accuracy of timestamps */
  guint32 snaplen;  /* max length of captured packets, in octets */
  guint32 network;  /* data link type */
};
```

According to the wireshark code, pcap file header include 7 fields. Details about that fields are given in the following table.




| Fields | Length  | Description |
| ------------ | ----------| ----------------------- |
| magic         | 4 Bytes | to record the start of file. default value is 0xa1b2c3d4 or 0xd4c3b2a1 |
| version_major | 2 Bytes | the major version number of the pcap file. default value is 0x0200     |
| version_minor | 2 Bytes | the minor version number of the pcap file. default value is 0x0400     |
| thiszone      | 4 Bytes | the local timestamps,  if use GMT, the value is usually 0x00000000     |
| sigfigs       | 4 Bytes | the accuracy of timestamps. the value is usually 0x00000000            |
| snaplen       | 4 Bytes | the max length of captured packets                                     |
| network       | 4 Bytes | data link type. such as Ethernet, 802.5 Token Ring, ARCnet and so on   |

#####  PCAP Packet Header 


```c
struct pcaprec_hdr {
  guint32 ts_sec;   /* timestamp seconds */
  guint32 ts_usec;  /* timestamp microseconds (nsecs for PCAP_NSEC_MAGIC) */
  guint32 incl_len; /* number of octets of packet saved in file */
  guint32 orig_len; /* actual length of packet */
};
```

According to the wireshark code, pcap file header include 4 fields. Details about that fields are given in the following table.


| Fields   | Length  | Description                                                                |
| -------- | ------- | -------------------------------------------------------------------------- |
| ts_sec   | 4 Bytes | the timestamp seconds                                                      |
| ts_usec  | 4 Bytes | the  timestamp microseconds                                                |
| incl_len | 4 Bytes | the number of octets of packet saved in file, not including packet header. |
| orig_len | 4 Bytes | the actual length of packet                                                |

