# Level01 Writeup

Like level00, there are no files in the home directory of the user. A similar scan using `find / -type f -user level01 2> /dev/null` and `find / -type f -user flag01 2> /dev/null` reveal nothing.

Looking at the `/etc/passwd` file show that the password hash for user flag00 is present. 
```
...
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
...
```
Using a password hash bruteforcer like john the ripper reveals that the password for the user flag01 is `abcdefg`
