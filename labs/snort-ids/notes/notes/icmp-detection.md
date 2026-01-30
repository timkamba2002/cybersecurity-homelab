# ICMP Detection with Snort

## Custom Rule
A local Snort rule was created to detect ICMP traffic entering the protected network.

```conf
alert icmp any any -> $HOME_NET any (msg:"ICMP Detected"; sid:1000001; rev:1;)
```
Attack Simulation
ICMP traffic was generated using the ping command from Kali Linux targeting the Metasploitable2 host.

ping 172.30.1.21
Results
Snort generated real-time alerts in the console

Alerts were written to /var/log/snort/snort.alert

Source and destination IPs matched expected lab hosts

Conclusion
This confirms Snort is properly configured, actively monitoring traffic, and capable of detecting basic network reconnaissance activity.
