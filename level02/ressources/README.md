We find a file named 'level02.pcap'.

```
level02@SnowCrash:~$ ls
level02.pcap
```

To analyze it, we transfer the file to our local machine using 'scp'.

```
scp -P 4242 level02@192.168.56.101:~/level02.pcap .
```

After entering the password, the transfer completes successfully.

**File Analysis in Wireshark:**

We open `level02.pcap` in Wireshark. Navigating to **Analyze -> Follow TCP Stream** and choosing **Hex Dump**, we see a series of hex values:

```
000000B9  66                                                 f
000000BA  74                                                 t
000000BB  5f                                                 _
000000BC  77                                                 w
000000BD  61                                                 a
000000BE  6e                                                 n
000000BF  64                                                 d
000000C0  72                                                 r
000000C1  7f                                                 .
000000C2  7f                                                 .
000000C3  7f                                                 .
000000C4  4e                                                 N
000000C5  44                                                 D
000000C6  52                                                 R
000000C7  65                                                 e
000000C8  6c                                                 l
000000C9  7f                                                 .
000000CA  4c                                                 L
000000CB  30                                                 0
000000CC  4c                                                 L
000000CD  0d                                                 .
```

**Interpretation of Hex Values:**

- Hex '7f' represents a delete character, which we ignore.
- Extracting the readable characters, we obtain the password: 'ft_waNDReL0L'.

```
'evel02@SnowCrash:~$ su flag02
Password:ft_waNDReL0L
Don't forget to launch getflag !

Success!

'lag02@SnowCrash:~$ getflag
Check flag.Here is your token :
```
