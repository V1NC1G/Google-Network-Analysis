# Google Cybersecurity Professional Certificate - Analyze Network Layer Communication

## Table of Contents

- [Objectives](#objectives)
- [Scenario](#scenario)
- [Guide](#guide)
- [Incident Report](#incident-report)
  - [Problem Summary](#problem-summary)
  - [Incident Analysis and Recommendation](#incident-analysis-and-recommendations)
- [Learning Experience](#learning-experience)

## Objectives

The objective for this activity is to analyze DNS and ICMP traffic in transit using data from a network protocol analyzer tool then identify which network protocol was utilized in assessment of the cybersecurity incident.

## Scenario

You are a cybersecurity analyst working at a company that specializes in providing IT services for clients. Several customers of clients reported that they were not able to access the client company website www.yummyrecipesforme.com, and saw the error “destination port unreachable” after waiting for the page to load.

You are tasked with analyzing the situation and determining which network protocol was affected during this incident. To start, you attempt to visit the website and you also receive the error “destination port unreachable.” To troubleshoot the issue, you load your network analyzer tool, tcpdump, and attempt to load the webpage again. To load the webpage, your browser sends a query to a DNS server via the UDP protocol to retrieve the IP address for the website's domain name; this is part of the DNS protocol. Your browser then uses this IP address as the destination IP for sending an HTTPS request to the web server to display the webpage The analyzer shows that when you send UDP packets to the DNS server, you receive ICMP packets containing the error message: “udp port 53 unreachable.”

```
13:24:32:192571 IP 192.51.100.15.52444 > 203.0.113.2.domain: 35084+ A?
yummyrecipesforme.com. (24)
13:24:36.098564 IP 203.0.113.2 > 192.51.100.15: ICMP 203.0.113.2
udp port 53 unreachable length 254

13:26:32.192571 IP 192.51.100.15.52444 > 203.0.113.2.domain: 35084+ A?
yummyrecipesforme.com. (24)
13:27:15.934126 IP 203.0.113.2 > 192.51.100.15: ICMP 203.0.113.2
udp port 53 unreachable length 320

13:28:32.192571 IP 192.51.100.15.52444 > 203.0.113.2.domain: 35084+ A?
yummyrecipesforme.com. (24)
13:28:50.022967 IP 203.0.113.2 > 192.51.100.15: ICMP 203.0.113.2
udp port 53 unreachable length 150
```

## Guide

In the `tcpdump` log, you find the following information:

1. The first two lines of the log file show the initial outgoing request from your computer to the DNS server requesting the IP address of yummyrecipesforme.com. This request is sent in a UDP packet.
2. The third and fourth lines of the log show the response to your UDP packet. In this case, the ICMP 203.0.113.2 line is the start of the error message indicating that the UDP packet was undeliverable to port 53 of the DNS server.
3. In the front of each request and response, you find timestamps that indicate when the incident happened. In the log, this is the first sequence of numbers displayed: 13:24:32.192571. This means the time is 1:24 PM., 32.192571 seconds.
4. After the timestamps, you will find the source and destination IP addresses. In the first line, where the UDP packet travels from your browser to the DNS server, this information is displayed as: 192.51.100.15 > 203.0.113.2.domain. The IP address to the left of the greater than symbol (>) is the source address, which in this example is your computer's IP address. The IP address to the right of the greater than symbol (>) is the destination IP address. In this case, it is the IP address for the DNS server: 203.0.113.2.domain. For the ICMP error response, the source address is 203.0.113.2 and the destination is your computers IP address 192.51.100.15.
5. After the source and destination IP addresses, there can be a number of additional details like the protocol, port number of the source and flags. In the first line of the error log, the query identification number appears as: 35084. The plus sign after the query identification number indicates there are flags associated with the UDP message. The "A?" indicates a flag associated with the DNS request for an A record, where an A record maps a domain name to an IP address. The third line displays the protocl of the response message to the browser: "ICMP", which is followed by an ICMP error message.
6. The error message, "udp port 53 unreachable" is mentioned in the last line. Port 53 is a port for DNS service. The word "unreachable" in the message indicates the UDP message requesting an IP address for the domain "www.yummyrecipesforme.com" did not go through the DNS server because no service was listening on the receiving DNS port.
7. The remaining lines in the log indicate that ICMP packets were sent two more times, but the same delivery error wase received both times.

## Incident Report

### Problem Summary

As part of the DNS protocol, the UDP protocol was used to contact the DNS server to get the IP address of the domain name _yummyrecipesforme.com_. The ICMP protocol was used to return an error message stating that "udp port 53 unreachable", meaning that there where issues contacting the DNS server.

The first two lines of the log event shows the UDP message going from the user's browser to the DNS server. The ICMP error response from the DNS is then shown in the third and fourth line of every log event, displaying "udp port 53 unreachable" which narrows the problem down to the DNS server having an issue. It can also be seen from the plus sign (+) after the query identification number because it indicates flags with the UDP message, and the "A?" symbol indicates flags with performing DNS protocol operations.

It is likely that the DNS server is not responding and is also supported by the flags associated with the outgoing UDP message and domain name retrieval.

### Incident Analysis and Recommendations

The incident occurred at 1:24 PM

Several clients raised a concern of not being able to access the company website yummyrecipesforme.com and that an error of "destination port unreachable" appears.

The security team is further investigating the issue so customers can access the website again.

From the initial investigation, we conducted packet sniffing tests using tcpdump and found out that the DNS port 53 is unreachable.

The next step of the investigation is to determine whether the DNS server is down or traffic to port 53 is blocked by the firewall.

The suspected cause of the problem is that the DNS server might be down due to a successful Denial of Service attack or a misconfiguration.

## Learning Experience

I learned a lot working on this activity specially reading log events from tcpdump and formulating possible causes for the problem in the activity which is an unreachable port. This also shows that knowing the uses for specific port numbers is useful when coming up with ideas on how to determine a possible cause for a network issue. This activity is also a great way to see how to make an incident report based on analysis of the log events presented by a network protocol analyzer tool.

This activity also brushed up on the process of how a DNS protocol works and flags associated with the UDP message and the DNS request.

Overall it is a great experince analyzing network protocol analyzer logs and I see it as a great stepping stone and a good foundation for network analysis and creating incident reports in the future in a work environment.
