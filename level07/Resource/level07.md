# Level07 Writeup

User level07 has a setuid (flag07) ELF Binary `level07` in the root of his home directory, using ghidra we can decompile `level07` and take a look at the c source code.

`level07`
``` c
int main(int argc,char **argv,char **envp) {
	char *env;
	int retval;
	char *buffer;
	gid_t gid;
	uid_t uid;
	char *command;
	__gid_t pgid;
	__uid_t puid;
  
 	pgid = getegid();
	puid = geteuid();
	setresgid(pgid,pgid,pgid);
	setresuid(puid,puid,puid);
	command = (char *)0x0;
	env = getenv("LOGNAME");
	asprintf(&command,"/bin/echo %s ",env);
	retval = system(command);
	return retval;
}
```

The binary `level07` uses an environment variable to construct a command. Checking the environment variables with `env` shows that indeed `LOGNAME` is set to `level07`. Setting the `env` like this `export LOGNAME=\$\(getflag\)` results in command subsitution when executing the `level07`, allowing us to get the flag.