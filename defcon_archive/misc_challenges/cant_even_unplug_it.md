## Challenge link
https://archive.ooo/c/cant_even_unplug_it/283/

## Challenge description
1. Text
You know, we had this up and everything. Prepped nice HTML5, started deploying on a military-grade-secrets.dev subdomain, got the certificate, the whole shabang. Boss-man got moody and wanted another name, we set up the new names and all. Finally he got scared and unplugged the server. Can you believe it? Unplugged. Like that can keep it secret...

2. The text file attached has the following text
Hint: these are HTTPS sites. Who is publicly and transparently logging the info you need?

Just in case: all info is freely accessible, no subscriptions are necessary. The names cannot really be guessed. 

## Solution
1. Since the description mentions https site and changing of the domains, we can try to lookup the TLS certificates issues for all the subdomains on https://crt.sh
2. Multiple domains show up on crt.sh. Now we have to lookup all the domains to find the flags. Since this is an old challenge, we have to use the wayback machine
3. When looking up "secret-storage.military-grade-secrets.dev" we find that is has a snapshot saved and it redirects to "https://forget-me-not.even-more-militarygrade.pw/". Now look this up on wayback machine and you have your flag. 
Flag: OOO{DAMNATIO_MEMORIAE} 
