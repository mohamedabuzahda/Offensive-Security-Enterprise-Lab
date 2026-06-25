nmap -sSCV -A -P dvwa.com      
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-06-22 23:04 UTC
Nmap scan report for dvwa.com (139.162.174.209)
Host is up (0.0022s latency).
Other addresses for dvwa.com (not scanned): 139.162.181.76 172.104.203.186 172.104.149.86 172.104.251.198 2a01:7e01::f03c:95ff:fe00:2692 2a01:7e01::f03c:95ff:feed:783e 2a01:7e01::2000:dff:fec1:5cdb 2a01:7e01::f03c:95ff:fe91:5d91 2a01:7e01::f03c:95ff:feeb:9d4b
rDNS record for 139.162.174.209: 139-162-174-209.ip.linodeusercontent.com
Not shown: 998 filtered tcp ports (no-response)
PORT    STATE SERVICE  VERSION
80/tcp  open  http     OpenResty web app server 1.29.2.5
|_http-cors: GET POST OPTIONS
|_http-server-header: openresty/1.29.2.5
|_http-title: Site doesn't have a title (application/octet-stream, text/plain).
| http-robots.txt: 1 disallowed entry 
|_/
443/tcp open  ssl/http OpenResty web app server 1.29.2.5
| ssl-cert: Subject: commonName=dvwa.com
| Subject Alternative Name: DNS:*.dvwa.com, DNS:dvwa.com
| Not valid before: 2026-05-09T16:29:13
|_Not valid after:  2026-08-07T16:29:12
|_http-cors: GET OPTIONS
|_http-server-header: openresty/1.29.2.5
| http-robots.txt: 1 disallowed entry 
|_/
|_http-title: Site doesn't have a title (application/octet-stream, text/plain).
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
No OS matches for host
Network Distance: 11 hops

TRACEROUTE (using port 80/tcp)
HOP RTT     ADDRESS
1   1.18 ms ip-10-114-0-175.eu-central-1.compute.internal (10.114.0.175)
2   3.08 ms 242.1.100.37
3   2.42 ms 100.100.2.16
4   3.81 ms ae8.r21.fra01.ien.netarch.akamai.com (23.210.54.172)
5   ...
6   3.26 ms ae3.r24.fra03.ien.netarch.akamai.com (23.197.79.119)
7   3.60 ms ae23.gw2-fra1.netarch.akamai.com (23.210.52.59)
8   ... 10
11  3.71 ms 139-162-174-209.ip.linodeusercontent.com (139.162.174.209)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 29.17 seconds


