nmap -sSCV -A -P dvwa.com 

ffuf -w /usr/share/wordlists/dirb/common.txt -u http://dvwa.com/FUZZ

nikto -h http://dvwa.com

ffuf -w wordlist.txt -u http://dvwa.com/FUZZ -fs 2

ffuf -w /usr/share/wordlists/dirb/common.txt -u http://dvwa.com/FUZZ -ac

curl http://dvwa.com/robots.txt

dirsearch -u http://dvwa.com -e php,html,txt