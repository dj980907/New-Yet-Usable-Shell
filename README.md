# New-Yet-Usable-Shell

This is a program that demonstrates my familiarity of the Linux programming environment and the shell.<br>
This is a basic shell that creates, destroy, and manages processes, handles signals and I/O redirection, invokes system calls. <br/>

## The Command
In each iteration, the user inputs a command terminated by the “enter” key (i.e., newline).<br/>
A command may contain multiple programs separated by the pipe (|) symbol. 

## Process termination and suspension
After creating the processes, this shell waits until all the processes have stopped running—either terminated or suspended. <br/>
Then, it prompts the user for the next command. 

## Signal handling
If a user presses `Ctrl-C` or `Ctrl-Z`, they don’t expect to terminate or suspend the shell. <br/>
Therefore, this shell ignores the following signals: SIGINT, SIGQUIT, and SIGTSTP. <br/>

## I/O redirection
Sometimes, a user would read the input to a program from a file rather than the keyboard, or send the output of a program to a file rather than the screen. <br/>
This shell program redirects the **standard input** (`STDIN`) and the **standard output** (`STDOUT`). <br/>
For simplicity, tis program does not redirect the **standard error** (`STDERR`).

### Input Redirection
Input redirection is achieved by a `<` symbol followed by a filename. For Example:
```
[nyush]$ cat < input.txt
```
If the file does not exist, this program prints the following error message to `STDERR` and prompt for the next command.<br/>
```
Error: invalid file
```
### Output Redirection
Output redirection is achieved by `>` or `>>` followed by a filename. For example:
```
[nyush]$ ls -l > output.txt
[nyush]$ ls -l >> output.txt
```
If the file does not exist, a new file should be created.<br/>
If the file already exists, redirecting with `>` overwrites the file (after truncating it), whereas redirecting with `>>` appends to the existing file.

### Pipe
The user may invoke n programs chained through (n - 1) pipes. <br/>
Each pipe connects the output of the program immediately before the pipe to the input of the program immediately after the pipe. For Example:
```
[nyush]$ cat shell.c | grep main | less
```

## Built-in Commands
There are four built-in commands in this program: `cd`, `jobs`, `fg`, and `exit`. 

### `cd <dir>`
This command changes the current working directory of the shell. <br/>
It takes exactly one argument: the directory, which may be an absolute or relative path. For Example:
```
[nyush local]$ cd bin
[nyush bin]$ █
```
If `cd` is called with 0 or 2+ arguments, your shell should print the following error message to `STDERR` and prompt for the next command.
```
Error: invalid command
```
If the directory does not exist, your shell should print the following error message to `STDERR` and prompt for the next command.
```
Error: invalid directory
```

### `jobs`
This command prints a list of currently suspended jobs to `STDOUT`, one job per line.<br/>
Each line has the following format: `[index] command`. For example:
```
[nyush]$ jobs
[1] ./hello
[2] /usr/bin/top -c
[3] cat > output.txt
[nyush]$ █
```
(If there are no currently suspended jobs, this command doees not print anything.)

A job is the whole command, including any arguments and I/O redirections.<br/>
A job may be suspended by `Ctrl-Z`, the `SIGTSTP` signal, or the `SIGSTOP` signal.<br/>
This list is sorted by the time each job is suspended (oldest first), and the index starts from 1.

The `jobs` command takes no arguments. <br/>
If it is called with any arguments, this program prints the following error message to `STDERR` and prompt for the next command.
```
Error: invalid command
```

### `fg <index>`
This command resumes a job in the foreground.<br/>
It takes exactly one argument: the job index, which is the number inside the bracket printed by the `jobs` command. For example:
```
[nyush]$ jobs
[1] ./hello
[2] /usr/bin/top -c
[3] cat > output.txt
[nyush]$ fg 2
```
The last command would resume `/usr/bin/top -c` in the foreground. <br/>

If `fg` is called with 0 or 2+ arguments, this program prints the following error message to `STDERR` and prompt for the next command.
```
Error: invalid command
```

If the job `index` does not exist in the list of currently suspended jobs, this program prints the following error message to `STDERR` and prompt for the next command.
```
Error: invalid job
```

### `exit`

This command terminates the shell.<br/>
However, if there are currently suspended jobs, this program will not terminate. <br/>
Instead, it prints the following error message to `STDERR` and prompt for the next command.
```
Error: there are suspended jobs

```
The `exit` command takes no arguments. <br/>
If it is called with any arguments, this program prints the following error message to `STDERR` and prompt for the next command.
```
Error: invalid command
```
If the `STDIN` of this program is closed (e.g., by pressing `Ctrl-D` at the prompt), this program terminates regardless of whether there are suspended jobs!
