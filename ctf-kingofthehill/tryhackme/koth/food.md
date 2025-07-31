# 🐧 Food

{% code overflow="wrap" %}
```
22/tcp   open  ssh
3306/tcp open  mysql
9999/tcp open  abyss
15065/tcp open  unknown
16109/tcp open  unknown
46969/tcp open  unknown

PORT     STATE  SERVICE VERSION
22/tcp   open   ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 280c0cd95a7dbee6f43ced1051494d19 (RSA)
|   256 17ce033bbb207809ab76c06d8dc4df51 (ECDSA)
|_  256 078a50b55b4aa76cc8b3a1ca77b90d07 (ED25519)
2206/tcp closed hpocbus
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.93%E=4%D=2/2%OT=22%CT=2206%CU=30049%PV=Y%DS=2%DC=I%G=Y%TM=65BD4
OS:B3B%P=x86_64-pc-linux-gnu)SEQ(SP=103%GCD=1%ISR=106%TI=Z%CI=Z%TS=A)OPS(O1
OS:=M509ST11NW7%O2=M509ST11NW7%O3=M509NNT11NW7%O4=M509ST11NW7%O5=M509ST11NW
OS:7%O6=M509ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)ECN(R=
OS:Y%DF=Y%T=40%W=F507%O=M509NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%R
OS:D=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%
OS:DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%
OS:O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=4
OS:0%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel


```
{% endcode %}

## User Accounts

```
dan?

root:
bread:
food:
pasta:pastaisdynamic mit stegseek
ramen:noodlesRTheBest
tryhackme:
```

## &#x20;Flags

* [ ] /home/tryhackme/flag7&#x20;
* [ ] /home/bread/flag
* [ ] /home/food/.flag
* [x] &#x20;/var/flag.txt
* [x] mysql:flag
*

```
mysql -h $IP -P 3306 -u root -p users (password: root)

#show tables
-> User

#select *from user
->ramen+pw
->flag:
```





