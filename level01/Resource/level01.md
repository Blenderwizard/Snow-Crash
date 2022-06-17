# Level01 Writeup
 
## Info

User level01 has no files within their home directory. Using the find command reveald nothing.
 
Looking at the /etc/passwd file shows that the password hash for user flag00 is present.
```
...
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
...
```

## /etc/passwd

Using a password hash brute force like john the ripper reveals that the password for the user flag01 is `abcdefg`, running getflag while logged in as flag01 gets us the flag.

