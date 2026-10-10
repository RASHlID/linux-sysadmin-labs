# Chapter 7 — Permissions Management

## Overview

Hands-on Linux system administration exercises completed while studying for the RHCSA (EX200), focusing on file ownership, access permissions, advanced permissions, and troubleshooting.

## Skills Practised

- Inspecting file and directory ownership with `ls -l` and `ls -ld`
- Changing ownership using `chown` and `chgrp`
- Managing recursive ownership with `chown -R`
- Understanding primary and supplementary groups
- Managing file permissions using symbolic and numeric `chmod`
- Troubleshooting file and directory access problems
- Understanding SUID permissions
- Configuring SGID directories for group ownership inheritance
- Understanding sticky-bit protection in shared directories
- Understanding default permissions using `umask`
- Managing immutable file attributes with `chattr` and `lsattr`

## Practical Exercises

### File Ownership and Groups
Practised changing user and group ownership, inspecting group membership, and understanding how effective group membership affects new files.

### Permission Management
Configured files and directories with different access requirements using numeric and symbolic permissions.

### Permission Troubleshooting
Investigated access problems involving group membership and parent directory permissions.

### Advanced Permissions
Examined SUID on `/usr/bin/passwd`, configured an SGID-enabled shared directory, and examined the sticky bit on `/tmp`.

### Default Permissions
Explored how `umask` affects the default permissions of newly created files and directories.

### Extended File Attributes
Applied the immutable attribute to a file and verified that modification and deletion were blocked.

## Commands Practised

`ls -l`, `ls -ld`, `chown`, `chgrp`, `chmod`, `groups`, `newgrp`, `usermod`, `find`, `namei`, `umask`, `chattr`, `lsattr`

## RHCSA Relevance

These exercises develop practical skills in managing Linux ownership, permissions, shared directories, and access troubleshooting.


