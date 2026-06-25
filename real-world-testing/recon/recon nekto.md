nikto -h http://dvwa.com
- Nikto v2.1.5
---------------------------------------------------------------------------
+ Target IP:          172.104.203.186
+ Target Hostname:    dvwa.com
+ Target Port:        80
+ Start Time:         2026-06-22 23:25:37 (GMT0)
---------------------------------------------------------------------------
+ Server: openresty/1.29.2.5
+ IP address found in the 'server' header. The IP is "1.29.2.5".
+ The anti-clickjacking X-Frame-Options header is not present.
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ Server leaks inodes via ETags, header found with file /robots.txt, fields: 0x69865a3a 0x134 
+ Uncommon header 'alt-svc' found, with contents: h3=":443"; ma=86400
+ Uncommon header 'strict-transport-security' found, with contents: max-age=0; includeSubDomains; preload
+ "robots.txt" contains 1 entry which should be manually viewed.
+ Uncommon header 'access-control-allow-origin' found, with contents: *
+ Uncommon header 'access-control-max-age' found, with contents: 86400
+ Uncommon header 'access-control-allow-methods' found, with contents: GET, POST, OPTIONS
+ DEBUG HTTP verb may show server debugging information. See http://msdn.microsoft.com/en-us/library/e8z01xdh%28VS.80%29.aspx for details.
+ OSVDB-28260: /_vti_bin/shtml.dll/_vti_rpc?method=server+version%3a4%2e0%2e2%2e2611: Gives info about server settings. CVE-2000-0413, CVE-2000-0709, CVE-2000-0710, http://www.securityfocus.com/bid/1608, http://www.securityfocus.com/bid/1174.
+ OSVDB-28260: /_vti_bin/shtml.exe/_vti_rpc?method=server+version%3a4%2e0%2e2%2e2611: Gives info about server settings.
+ OSVDB-3092: /_vti_bin/_vti_aut/author.dll?method=list+documents%3a3%2e0%2e2%2e1706&service%5fname=&listHiddenDocs=true&listExplorerDocs=true&listRecurse=false&listFiles=true&listFolders=true&listLinkInfo=true&listIncludeParent=true&listDerivedT=false&listBorders=fals: We seem to have authoring access to the FrontPage web.
+ OSVDB-3092: /_vti_bin/_vti_aut/author.exe?method=list+documents%3a3%2e0%2e2%2e1706&service%5fname=&listHiddenDocs=true&listExplorerDocs=true&listRecurse=false&listFiles=true&listFolders=true&listLinkInfo=true&listIncludeParent=true&listDerivedT=false&listBorders=fals: We seem to have authoring access to the FrontPage web.
+ 6544 items checked: 23 error(s) and 14 item(s) reported on remote host
+ End Time:           2026-06-22 23:33:21 (GMT0) (464 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested