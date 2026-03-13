ip 10.129.2.137

![[Pasted image 20260307165801.png]]
Found a website on http (port 80) and changed hosts to see http://facts.htb

Gobuster finds a /admin, i can create and account and login

![[Pasted image 20260307171311.png]]

Camaleon v2.9.0 has an associated CVE ->  CVE-2025-2304
using this poc : https://github.com/d3vn0mi/cve-2025-2304-poc 
![[Pasted image 20260307172603.png]]
![[Pasted image 20260307172651.png]]



![[Pasted image 20260307174235.png]]
![[Pasted image 20260307174306.png]]
could be usefull ...
![[Pasted image 20260307174332.png]]

using ssh2john and john, i manage to get an ssh acces
![[Pasted image 20260307180111.png]]
`dragonballz` is our password !

Using `sudo -l` we find a strange binary
![[Pasted image 20260307181214.png]]

facter has a privilege escalation issue cause i can execute scripts

![[Pasted image 20260307182506.png]]
there it is