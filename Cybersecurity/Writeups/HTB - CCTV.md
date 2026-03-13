ip 10.129.4.96

nmap reveal a website at http://cctv.htb, it seems to be a physical security provider.
It seems to be based on Zoneminder.

I find a couple CVE but both patched on my version

with a bit of luck i find that admin:admin allows me to log in

here are the users
![[Pasted image 20260309112703.png]]

Actually i find a CVE patched  and the reintroduced [CVE-2026-27470](https://github.com/kocaemre/CVE-2026-27470)



poc : http://cctv.htb/zm/index.php?view=request&request=event&action=removetag&tid=1
-> sqlmap ->

![[Pasted image 20260309152617.png]]
`$2y$10$prZGnazejKcuTv5bKNexXOgLyQaok0hq07LW7AJ/QNqZolbXKfFG.`


X1l9fx1ZjS7RZb