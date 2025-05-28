# shell

This project was completed as part of **CSCI0330: Introduction to Computer Systems** at Brown University. It implements a custom Unix-style shell in C that supports command execution, job control, signal handling, and I/O redirection.

**Note:** Due to course policies, the full codebase cannot be shared publicly. Please contact me (Brendan Rathier) directly if you would like access for educational purposes.


## Overview

This shell replicates essential functionality found in traditional Unix shells. It supports both built-in and external commands, background and foreground job execution, terminal signal control, and input/output redirection. The shell is designed to read user input in a continuous REPL, and includes extensive error checking to handle improper arguments or syntax.

### Core Features

- Execution of external programs with argument parsing
- Built-in commands:
  - `cd`: Change directory
  - `rm`: Remove file
  - `ln`: Create hard link
  - `exit`: Exit the shell
  - `jobs`: List background jobs
  - `fg <JID>`: Resume a background job in the foreground
  - `bg <JID>`: Resume a stopped job in the background
- Input/output redirection using `<` and `>`
- Foreground and background process execution (`&`)
- Job list management with globally tracked JIDs
- Signal handling (e.g., `SIGINT`, `SIGTSTP`, `SIGCHLD`) and proper terminal control
- Error handling for incorrect command syntax, missing arguments, and invalid job IDs

---

## Implementation Highlights

- **Command Parsing**: Supports parsing of commands with arguments, redirection symbols, and background execution indicators.
- **Built-in Handling**: The shell detects and executes built-in commands without forking, managing edge cases such as invalid paths or missing arguments.
- **Forking and Execution**: For non-built-in commands, the shell forks child processes. It assigns foreground jobs terminal control and tracks background jobs in a global job list.
- **Job Control**: Maintains a list of background jobs with unique job IDs. Allows users to resume jobs using `fg` or `bg`.
- **Signal Handling**:
  - `SIGCHLD`: Reaps terminated or stopped background processes
  - `SIGINT` / `SIGTSTP`: Properly forwarded to foreground processes
- **Reaping Logic**:
  - `reap_foreground`: Waits on a foreground job and handles any state transitions
  - `reap_background`: Iterates through jobs at the start of each REPL cycle to reap exited or stopped background processes
