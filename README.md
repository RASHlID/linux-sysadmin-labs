# Linux System Administration Labs

Hands-on Linux system administration labs completed while studying for the RHCSA (EX200).

## Progress

- [x] Chapter 2 – Essential Shell Skills
- [x] Chapter 3 – Essential File Management Tools
- [x] Chapter 4 - Working with Text Files
- [x] Chapter 5 - Connecting to a Linux Server

## Skills Practised

- Linux command line and Bash shell
- Filesystem navigation and file management
- I/O redirection and pipes
- Environment variables
- vim and nano
- Linux documentation and man pages
- Hard and symbolic links
- tar archives and compression

## Labs

### Chapter 2 – Essential Shell Skills

Topics covered:

- Executing commands
- Bash completion
- Command history
- STDIN, STDOUT and STDERR
- I/O redirection
- Pipes
- Shell variables
- Environment variables
- PATH
- vim and nano

---

### Chapter 3 – Essential File Management Tools

Hands-on practice with Linux file management, links, archives and compression.

#### Skills Practised

- Linux filesystem hierarchy and navigation
- Absolute and relative paths
- File/directory management with `mkdir`, `touch`, `cp`, `mv`, and `rm`
- Wildcards: `*`, `?`, `[ ]`
- Hidden and unusual filenames
- Hard links, symbolic links and inodes
- Creating, inspecting and extracting tar archives
- Extracting archives to specific locations with `-C`
- Appending and updating existing archives
- gzip, bzip2 and xz compression
- Creating compressed archives directly with `tar`

#### Archive & Compression Practice

Created and inspected tar archives:

```bash
tar cvf backups/config-backup.tar config/
tar tf backups/config-backup.tar
tar xf backups/config-backup.tar
```

Restored backups to a specific location:

```bash
tar xf backups/config-backup.tar -C restore/
```

Added and updated archive contents:

```bash
tar rf backups/config-backup.tar data/users.txt
tar uf backups/config-backup.tar data/users.txt
```

Created compressed archives:

```bash
tar czvf backup.tar.gz config/
tar cjvf backup.tar.bz2 config/
tar cJvf backup.tar.xz config/
```

#### Links

Practised hard and symbolic links:

```bash
ln original.txt hardlink.txt
ln -s original.txt symlink.txt
```

Used inode information to understand the difference between hard links and symbolic links.

#### Mini Sysadmin Project

Built a backup and recovery lab containing:

```text
rhcsa-backup/
├── config/
│   ├── app.conf
│   └── database.conf
├── data/
│   └── users.txt
├── backups/
└── restore/
```

Used the environment to practise creating archives, inspecting backup contents, restoring files, updating existing archives and applying different compression formats.

#### Key Takeaways

- `tar` archives files; gzip, bzip2 and xz compress data.
- `tar tf` can inspect an archive before extraction.
- `-C` allows controlled extraction to another directory.
- Hard links share an inode, while symbolic links reference another pathname.
- gzip uses `z`, bzip2 uses `j`, and xz uses `J` with `tar`.
---

# Chapter 4 – Working with Text Files

Hands-on practice with Linux text processing, searching, regular expressions, and command pipelines.

## Skills Practised

- Viewing text files with `cat` and `less`
- Navigating and searching inside `less`
- Displaying specific lines with `head` and `tail`
- Monitoring files with `tail -f`
- Counting lines, words, and bytes with `wc`
- Extracting fields with `cut`
- Sorting text and fields with `sort`
- Combining multiple commands using pipes
- Searching text with `grep`
- Case-insensitive and inverse matching with `grep`
- Using regular expressions
- Using line anchors `^` and `$`
- Using character sets `[ ]` and wildcards `.`
- Using regex multipliers such as `*`
- Extracting fields with `awk`
- Displaying, replacing, and deleting text with `sed`

## Lab Structure

Created a text-processing lab environment:

```text
chapter4lab/
├── backup/
├── data/
│   ├── server.log
│   └── users.txt
└── reports/
# Chapter 5 – Connecting to a Linux Server

Hands-on practice with Linux sessions, remote administration, secure file transfers, synchronization, and SSH key-based authentication.

## Skills Practised

- Identifying terminal sessions with `tty`
- Viewing logged-in users and sessions with `w`
- Understanding TTYs and pseudo-terminals (`/dev/pts`)
- Checking services with `systemctl`
- Verifying the OpenSSH server (`sshd`)
- Connecting to remote systems with SSH
- Understanding SSH host fingerprints and `known_hosts`
- Troubleshooting SSH connections with verbose mode
- Securely transferring files with `scp`
- Using interactive SFTP sessions
- Synchronizing files and directories with `rsync`
- Creating and using SSH public/private key pairs
- Installing public keys with `ssh-copy-id`
- Understanding `authorized_keys`
- Using SSH public-key authentication

## SSH and Remote Sessions

Verified that the OpenSSH server was running and listening for connections:

```bash
systemctl status sshd
```

Connected to the server using SSH:

```bash
ssh rashid@192.168.0.100
```

Used `tty` and `w` to inspect sessions and observe how SSH creates a new pseudo-terminal under `/dev/pts`.

Used SSH verbose mode to troubleshoot the connection process:

```bash
ssh -v rashid@192.168.0.100
```

Learned how SSH stores trusted server identities in:

```text
~/.ssh/known_hosts
```

and how host-key fingerprints help detect unexpected changes to a server's identity.

## Secure File Transfer with SCP

Transferred files securely over SSH using `scp`.

Uploaded a local file to a remote directory:

```bash
scp server-report.txt rashid@192.168.0.100:/tmp
```

Downloaded a remote file back to the local system:

```bash
scp rashid@192.168.0.100:/tmp/server-report.txt ~/linux-sysadmin-labs/chapter5lab/data/
```

Verified transferred files using `ls` and `cat`.

## SFTP

Opened an interactive SFTP session:

```bash
sftp rashid@192.168.0.100
```

Practised working with both local and remote directories:

```text
pwd     - remote working directory
lpwd    - local working directory

ls      - list remote files
lls     - list local files

cd      - change remote directory
lcd     - change local directory
```

Uploaded and downloaded files using:

```text
put     - local → remote
get     - remote → local
```

## File Synchronization with rsync

Used `rsync` to synchronize directories:

```bash
rsync -av transfer/ backup/
```

Observed that the initial synchronization transferred the files, while running the same command again avoided retransferring unchanged files.

Also practised relative paths while synchronizing from inside a directory:

```bash
rsync -av ../transfer/ ../backup/
```

## SSH Key Authentication

Inspected an existing Ed25519 SSH key pair:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

Installed the public key on the SSH server:

```bash
ssh-copy-id rashid@192.168.0.100
```

Verified that the public key was stored in:

```text
~/.ssh/authorized_keys
```

Confirmed with SSH verbose output that authentication was performed using the public key:

```text
Authenticated ... using "publickey"
```

The private key remained on the client system while the public key was installed on the server.

## Lab Structure

```text
chapter5lab/
├── backup/
│   ├── app.conf
│   ├── final-check.txt
│   ├── notes.txt
│   ├── server-report.txt
│   └── users.txt
├── data/
│   └── server-report.txt
└── transfer/
    ├── app.conf
    ├── final-check.txt
    ├── notes.txt
    ├── server-report.txt
    └── users.txt
```

## Key Takeaways

- `tty` identifies the terminal associated with the current shell.
- SSH sessions normally receive their own pseudo-terminal under `/dev/pts`.
- `systemctl status sshd` can verify whether the SSH server is running.
- SSH normally uses TCP port 22.
- SSH host keys identify servers, while `known_hosts` records previously trusted server identities.
- `ssh -v` provides detailed information for troubleshooting SSH connections.
- `scp` performs secure one-shot file transfers over SSH.
- SFTP provides an interactive environment for secure file transfers.
- `rsync` efficiently synchronizes files and avoids retransferring unchanged data.
- `.` represents the current directory and `..` represents its parent directory.
- SSH private keys must remain private; public keys can be installed on remote servers.
- `ssh-copy-id` installs a public key into the remote user's `authorized_keys`.
- SSH public-key authentication allows the server to authenticate a user without requiring the remote account password.
