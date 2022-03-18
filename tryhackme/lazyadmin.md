# LazyAdmin
## Nmap scan
Nmap scan shows 2 open ports : 22 and 80. Since ssh exploit is unlikely, let's look at port 80, which shows apache server running

## Enumerating port 80
1. When we visit port 80, we see the default apache server page with php installed. Since this does not give us useful hints, let's use gobuster to discover directories.
2. Gobuster returns one useful route '/content'. On visiting the route, we get to know that the website is using sweetrice cms. So let's find exploits for it.
<pic>

## Exploiting sweetrice cms
1. On searching for exploits, I came across [this](https://www.exploit-db.com/exploits/40718) article which says that mysql backup can be obtained from the route "/inc/mysql\_backup"
2. When we visit the route, we find a mysql backup which contains the username "manager" and password "Password123". I tried to ssh using these creds but it did not work. So that means we have to login to the site using these creds.
3. Next, I found [this](https://www.exploit-db.com/exploits/40716) arbitrary file upload exploit. We put in a reverse shell, username and password and boom we have a shell.
4. Now we are in the system as the user 'www-data' and we can see the user 'itguy' in the home directory. We can read their files, so we get the user flag. 
5. Now since we do not have a tty, we need to a bash shell so that we are not limited in the commands that we can run. As python is installed on the victim machine, we get a bash shell with the following trick : "python -c 'import pty; pty.spawn("/bin/bash")'"

## Privesc
1. In the itguy's folder, there is a file called backup.pl, which is owned by root. It executed the script /etc/copy.sh
2. When we run 'sudo -l', we see that we can run /usr/bin/perl and the backup.pl without root password.
3. So we add the following payload to /etc/copy.sh
```bash
cp /bin/bash /tmp/bash;chmod +xs /tmp/bash
```
4. Now we execute the script as follows
```bash
sudo /usr/bin/perl /home/itguy/backup.pl
```
5. Now we can get the root shell by running the command
```bash
sudo /tmp/bash -p
```
