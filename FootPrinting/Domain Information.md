# Domain Information

### Certificate Transparency

`curl -s https://crt.sh/\?q\=inlanefreight.com\&output\=json | jq .`

**filtered by the unique subdomains**  
`curl -s https://crt.sh/\?q\=inlanefreight.com\&output\=json | jq . | grep name | cut -d":" -f2 | grep -v "CN=" | cut -d'"' -f2 | awk '{gsub(/\\n/,"\n");}1;' | sort -u`

* * *

### Company Hosted Servers
`for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f1,4;done`

* * *

### Shodan - IP List
`for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f4 >> ip-addresses.txt;done`

`for i in $(cat ip-addresses.txt);do shodan host $i;done`

* * *

### DNS Records
`dig any inlanefreight.com`

**A Records:** Show IP addresses linked to (sub)domains. Here, we see one we already know.  
**MX Records:** Reveal mail servers. In this case, Google handles email, so we can skip this for now.  
**NS Records:** Identify name servers that resolve FQDN to IP addresses. These often indicate the hosting provider.  
**TXT Records:** Contain verification keys and security info like SPF, DMARC, and DKIM. These can give valuable insights into email origin verification and security.

By reviewing these records, we apply the principles of seeing what’s visible, identifying valuable information, and using it to understand the target better.

* * *
