DNS Query Detection Test (Sysmon Event ID 22)

After enabling full DNS monitoring in Sysmon by removing all <DnsQuery onmatch="exclude"> blocks, I tested detection using a controlled command:

    nslookup totallynotmalware.evil

Sysmon successfully logged the DNS query under Event ID 22. The event shows PowerShell.exe performing the lookup, the suspicious domain name, process ID, user context, and DNS query status. This confirms that Sysmon is correctly configured to detect unusual outbound DNS behavior that could indicate malware or command-and-control (C2) activity.

# DNS Exfiltration & Suspicious Lookup Detection (Sysmon Event ID 22)

## Overview
This test demonstrates my ability to detect suspicious DNS lookups using Sysmon Event ID 22 and a custom Sysmon configuration. The goal was to confirm that my Sysmon setup is correctly logging DNS queries from specific processes (such as PowerShell), even when those domains are abnormal or potentially malicious.

---

## What the Test Was
I performed a controlled DNS lookup to a fake, clearly abnormal domain:


This kind of domain mimics what malware often uses for:
- command-and-control communication  
- data exfiltration  
- staging of attacks  
- initial beaconing  

The purpose was to verify that Sysmon:
1. Captured the DNS request  
2. Logged the requesting process  
3. Recorded the domain  
4. Provided insight into whether the request was successful or blocked  

---

## How I Triggered the DNS Logging
I opened **PowerShell** and ran:

nslookup totallynotmalware.evil

This forces Windows to perform a DNS lookup, which Sysmon should log (if the config is correct).

I then used Sysmon’s updated configuration, where I removed all `<DnsQuery onmatch="exclude">` blocks so Sysmon would log EVERY DNS query. This allowed Event ID 22 to be recorded when I ran the test.
