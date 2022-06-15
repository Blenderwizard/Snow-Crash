# Level00 Writeup

An initial look at the home directory of level00 shows nothing of interest. A scan using the find command, `find / -type f -user level00 2> /dev/null`, reveales nothing of ineterest. However a scan looking for files created/owned by flagholder user flag00 `find / -type f -user flag00 2> /dev/null` reveals one file of interest: `/usr/sbin/john`.

`/usr/sbin/john` contains the text contents `cdiiddwpgswtgt`, however this does not work as a password for the flag00 user. Preforming a cipher analysis from [dcode](https://www.dcode.fr/cipher-identifier) show that this is a simple ROT15 cipher.
`nottoohardhere` is the password for the flag00 user.

