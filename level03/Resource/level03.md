# Level03 Writeup

## Info

User level03 has a setuid (flag03) ELF binary `level03` in their home directory.
 
## `level03`

Executing the file gives us the following output: Exploit me. Using the strings command we can see that one of the strings is "/usr/bin/env echo Exploit me". Which is most likely what is displaying the string "Exploit me". Meaning that the program uses either execve or system to execute echo`to display the text.
 
This is where the security vulnerability lies. Since echo is not hard coded as a path, we can define our own `echo` script and add it to the PATH env variable, executing our own script instead of the normal echo. We create the following file named `echo`:
``` bash
#!/bin/bash
 
/bin/bash
```

## Victory
 
After editing the PATH variable with `export PATH=/location/of/our/echo:$PATH`, executing `level03` drops us into a shell for the user flag03. Running getflag gets us the flag.
 

