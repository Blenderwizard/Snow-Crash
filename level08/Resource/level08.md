# Level08 writeup

User level08 has setuid (flag08) ELF binary `level08` and a plaintext file with no read permisions owned by flag08 `token`. Using a decompiler like ghidra we can see the c source code of `level08`

`level08`
``` c
int main(int argc,char **argv,char **envp) {
	char *comparer;
	int f;
	size_t size;
	ssize_t ret;
	int in_GS_OFFSET;
	int fd;
	int rc;
	char buf [1024];
	undefined data [1024];
	int bufferoverflowcheck;
  
	bufferoverflowcheck = *(int *)(in_GS_OFFSET + 0x14);
	if (argc == 1) {
		printf("%s [file to read]\n",*argv);
		exit(1);
	}
	comparer = strstr(argv[1],"token");
	if (comparer != (char *)0x0) {
		printf("You may not access \'%s\'\n",argv[1]);
	exit(1);
	}
	f = open(argv[1],0);
	if (f == -1) {
		err(1,"Unable to open %s",argv[1]);
	}
	size = read(f,data,0x400);
	if (size == 0xffffffff) {
		err(1,"Unable to read fd %d",f);
	}
	ret = write(1,data,size);
	if (bufferoverflowcheck != *(int *)(in_GS_OFFSET + 0x14)) {
		__stack_chk_fail();
	}
	return ret;
}
```

As we can see, `level08` attempts to read a file pass as a comand line argument but if the name maches token, it refuses to read it. The solution is simple, rename `token` to anything else like this `mv token tok` and your able to read the password of flag08. Login to flag08 and run getflag for the flag!