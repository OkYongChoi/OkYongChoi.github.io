---
title: "profile vs bashrc?"
toc: true
categories:
  - 
tags:
  - linux
  - shell
---

# profile vs bashrc
- Files encountered when editing PATH or alias, etc. in Linux.
- profile specifies environment variable.
- bashrc specifies an alias.

## ~/.bash_profile vs /etc/profile

- ~/.bash_profile is a local file and is executed only when the corresponding user logs in.
- /etc/profile is a global file and is executed when all users log in.
- Also, ~/.bash_profile does not apply to shells such as zsh and applies only to the bash shell.
- Whereas /etc/profile is always applied regardless of any shell.

## ~/.bashrc vs /etc/bashrc

- Unlike profile, there is no login process, so it is classified as *the user who runs the shell*.
- ~/.bashrc is executed when the corresponding user executes a shell,
- /etc/bashrc is executed whenever a shell is executed by all users.

# Login Shell vs Non Login Shell

## Login Shell
-   This means that a login is required to run the shell.
-   This is applicable when connecting with ssh or entering another account with the su command.

## Non-Login Shell
- A case in which a login is not required to run the shell. That is, it means any situation in which the shell is
- Applicable when opening a terminal in the GUI or executing bash again with the bash command
- Non-login shell includes login shell.
- e.g. /etc/bashsrc, ~/.bashrc

## Example
- When the login shell is executed, both the *profile* and *bashrc* files are executed.
- Only *bashrc* is executed when non-login shell is executed

# Summary
Now can you distinguish the following things?

- ~/.bash_profile
- /etc/profile
- ~/.bash_rc
- /etc/bashsrc