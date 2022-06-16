# Level10 writeup

## Info

User level10 has a setuid (flag10) ELF binary `level10` and a read protected file `token` owned by user flag10. `level10` program takes 2 arguments, a file to send and a host on port 6969 to send the file too. Attempting to send `token` fails, since `level10` checks if the user executing the program has the permisions to read the file, erroring out if they don't.

There are 2 problems to solve, getting the data that is sent to the host and getting `level10` to read the contents of `token`.

## Reading contents of the file sent

This can be easily solved, simply modify a webserver like nginx to listen on port 6969 and monitor packets using wireshark.

nginx.conf
``` conf
...
server {
	server_name temp;
	listen		6969;
}
...
```
## Sending the contents of `token`

Simply by swaping the names of a file we create and `token` very fast, we can cause a race condtion. This condition is when program sees that we have the correct permisions to read the file and the time it reads the content and sends it. If when the file is checked it checks our file and when the program reads the file it reads `token`, then we have won the race.

Example script to swap files:
``` bash
while [ 1 ]
do
        mv token temp
        mv file token
        mv temp file
done
```

## Victory

After winning the race, we can use the contents of `token` as a password for flag10 and get the flag.