# Command Injection
This vulnerability exists because applications often use functions in programming languages such as PHP, Python and NodeJS to pass data to and to make system calls on the machine’s operating system. For example, taking input from a field and searching for an entry into a file. You can often determine whether or not command injection may occur by the behaviours of an application.

Applications that use user input to populate system commands with data can often be combined in unintended behaviour. For example, the shell operators `;`, `&` and `&&` will combine two (or more) system commands and execute them both

## Command Injection can be detected in mostly one of two ways:
1. Blind Command Injection.
     This type of injection is where there is no direct output from the application when testing payloads. You will have to investigate the behaviours of the application to determine whether or not your payload was successful.

2. Verbose Command Injection.
     This type of injection is where there is direct feedback from the application once you have tested a payload. For example, running the whoami command to see what user the application is running under. The web application will output the username on the page directly.

## Detecting Blind Command Injection
For this type of command injection, we will need to use payloads that will cause some time delay. For example, the ping and sleep commands are significant payloads to test with. Using `ping` as an example, the application will hang for `x` seconds in relation to how many pings you have specified.
Another method of detecting blind command injection is by forcing some output. This can be done by using redirection operators such as `>`. For example, we can tell the web application to execute commands such as `whoami` and redirect that to a file. We can then use a command such as `cat` to read this newly created file’s contents.

The `curl` command is a great way to test for command injection. This is because you are able to use `curl` to deliver data to and from an application in your payload.
eg: `curl http://vulnerable.app/process.php%3Fsearch%3DThe%20Beatles%3B%20whoami`

# Detecting Verbose Command Injection
Detecting command injection this way is arguably the easiest method of the two. Verbose command injection is when the application gives you feedback or output as to what is happening or being executed.

# Input sanitisation
Sanitising any input from a user that an application uses is a great way to prevent command injection. This is a process of specifying the formats or types of data that a user can submit. For example, an input field that only accepts numerical data or removes any special characters such as > ,  & and /.

# Bypassing Filters
Applications will employ numerous techniques in filtering and sanitising data that is taken from a  user's input. These filters will restrict you to specific payloads; however, we can abuse the logic behind an application to bypass these filters. For example, an application may strip out quotation marks; we can instead use the hexadecimal value of this to achieve the same result.


# Methodology
1. search for the input field, enter the valid input with the command
   eg: `hello; whoami` & `hello && whoami` & `hello & whoami` "hello" -> is the valid input and "whoami" is the command you want to execute. 

## Useful Payloads

I have compiled some valuable payloads for both Linux & Windows into the tables below.
`https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Command%20Injection/Intruder/command_exec.txt`  `https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Command%20Injection/Intruder/command-execution-unix.txt`

## Linux Payloads

| Payload | Description |
|--------|-------------|
| `whoami` | See what user the application is running under. |
| `ls` | List the contents of the current directory. You may be able to find files such as configuration files, environment files (tokens and application keys), and other valuable information. |
| `ping` | Causes the application to hang. Useful for testing blind command injection. |
| `sleep` | Useful for testing blind command injection when `ping` is not installed. |
| `nc` | Netcat can be used to spawn a reverse shell on the vulnerable application, allowing further enumeration, file access, and potential privilege escalation. |

## Windows Payloads

| Payload | Description |
|--------|-------------|
| `whoami` | See what user the application is running under. |
| `dir` | List the contents of the current directory. You may be able to find files such as configuration files, environment files (tokens and application keys), and other valuable information. |
| `ping` | Causes the application to hang. Useful for testing blind command injection. |
| `timeout` | Causes the application to hang. Useful for blind command injection testing when `ping` is unavailable. |
