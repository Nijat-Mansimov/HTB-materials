Executing BloodHound.py

```shell
sudo bloodhound-python -u 'forend' -p 'Klmcargo2' -ns 172.16.5.5 -d inlanefreight.local -c all 
```

Viewing the Results
```shell
nijatmansimov@htb[/htb]$ ls

20220307163102_computers.json  20220307163102_domains.json  20220307163102_groups.json  20220307163102_users.json
```

Start GUI server
```shell
sudo neo4j start
```

user == neo4j / pass == HTB_@cademy_stdnt!

Upload the Zip File into the BloodHound GUI

<img width="1809" height="1068" alt="image" src="https://github.com/user-attachments/assets/333c96a1-2647-42be-842e-a2a602b12292" />

<img width="1809" height="1073" alt="image" src="https://github.com/user-attachments/assets/1255170a-242a-4675-a72b-02b437b1d26f" />
