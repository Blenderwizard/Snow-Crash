# Level07 Writeup
 
## Info
 
User level07 has a setuid `flag07` ELF Binary `level07` in the root of his home directory, using ghidra we can decompile `level07` and take a look at the c source code.
 
`level07`
``` c
int main(int argc,char **argv,char **envp) {
   char *env; // Ghidra mess
   int retval;
   char *buffer;
   gid_t gid;
   uid_t uid;
   char *command;
   __gid_t pgid;
   __uid_t puid;
   pgid = getegid(); // Making sure setuid works
   puid = geteuid();
   setresgid(pgid,pgid,pgid);
   setresuid(puid,puid,puid);
   command = (char *)0x0;
   env = getenv("LOGNAME"); // Get env LOGNAME and store it
   asprintf(&command,"/bin/echo %s ",env); // constructs a command with /bin/echo and contents of LOGNAME
   retval = system(command); // Executes command
   return retval;
}
```
 
## Victory
 
The binary `level07` uses an environment variable to construct a command. Checking the environment variables with `env` shows that indeed `LOGNAME` is set to `level07`.

We can change the contents of `LOGNAME` using the command `export LOGNAME=\$\(getflag\)`. Now when we execute `level07` we get the flag.