# Level00 Writeup
 
## Info

User level00 has no files within his home directory, using the find command (`find / -type f -user flag00 2> /dev/null`) we can locate one plaintext file `john`.

## jhon
 
`john` contains "cdiiddwpgswtgt", however this does not work as a password for the flag00 user. Performing a cipher analysis from [dcode](https://www.dcode.fr/cipher-identifier) shows that this is a simple ROT15 cipher.
"nottoohardhere" is the password for the flag00 user.