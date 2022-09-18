# Level01 Writeup
 
## Info

`level01` has no files within their home directory. Using `ls -la` confirms this, and using `find / -type f -user flag01 2> /dev/null` also reveales nothing.

Looking at the `/etc/passwd` file shows that there is information for the password of user `flag00`.
```
...
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
...
```

## /etc/passwd and jhon the ripper

Using a password brute forcer like john the ripper solves the password as `abcdefg`.

## Victory

Logging into `flag01` with the password `abcdefg` and running getflag gets us the flag.

