# Linux System Administration Labs

Hands-on Linux system administration labs completed while studying for the RHCSA (EX200).

## Progress

- [x] Chapter 2 – Essential Shell Skills
- [x] Chapter 3 – Essential File Management Tools
- [ ] Chapter 4

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
