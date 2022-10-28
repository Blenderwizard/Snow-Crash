# Level00 Writeup
 
## Info

User level00 has no obvious files within their home directory. With `ls -la` we can confirm that there are no files of interest. 

Using the command `find / -type f -user flag00 2> /dev/null`, we can search the entire system for files that belong to the user flag00 that we have access to. We find a single plaintext file called `john`.

## jhon
 
`john` contains "cdiiddwpgswtgt", however this isn't the password for `flag00`. Performing a cipher analysis from [dcode](https://www.dcode.fr/cipher-identifier) shows that this is a simple ROT15 cipher. We can use dcode again too solve the cipher, giving us the password `nottoohardhere` for `flag00`.

## Victory
Login to user `flag00` with the password `nottoohardhere` and run `getflag`.