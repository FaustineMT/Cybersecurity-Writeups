# IP - Time To Live

- **Category:** Network (Root-Me)
- **Difficulty:** Easy
- **Objective:** Find the TTL (Time To Live) used to reach the targeted host in an ICMP exchange.

## 1. Traffic Analysis
By opening the network capture file in Wireshark, we analyze the ICMP exchange to map out the traceroute pattern and identify communication endpoints.
- **Source IP:** `ch11.challenge01.root-me.org`
- **Destination IP** `challenge01.root-me.org`

## 2. Exploitation
Filtering and tracking the incrementing TTL values across the captured packets:
  ```bash
  ip.dst == 198.173.244.32```
Observing that the TTL value increments sequentially (1, 2, 3 ...) alongside "Time to Live Exceeded" ICMP messages, indicating a traceroute command execution. Scrolling down to the final packet that successfully reaches the target host.

**Flag:** `13`
  
## 3. Key Takeaways & Remediation
- **Concept:**The Time To Live (TTL) field in IP headers decreases by 1 at each router hop. Diagnostic tools like traceroute leverage this decrementing behavior to map out network paths and determine the number of hops to a destination.
