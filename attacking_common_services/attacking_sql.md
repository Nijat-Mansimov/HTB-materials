Enumeration
```shell
nmap -Pn -sV -sC -p1433 10.10.10.125
```

MySQL - Connecting to the SQL Server
```shell
mysql -u julio -pPassword123 -h 10.129.20.13
```

Sqlcmd - Connecting to the SQL Server
```shell
sqlcmd -S SRVMSSQL -U julio -P 'MyPassword!' -y 30 -Y 30
```

```shell
mssqlclient.py -p 1433 julio@10.129.203.7
```

```sql
1> SELECT name FROM master.dbo.sysdatabases
2> GO

name
--------------------------------------------------
master
tempdb
model
msdb
htbusers
```
XP_CMDSHELL
```sql
xp_cmdshell 'whoami'
```






























