# Chapter 10 – Managing Processes

RHCSA (EX200) | Red Hat Enterprise Linux 10

## Topics Covered

- Understanding Linux processes, PIDs and PPIDs
- Managing foreground and background jobs
- Monitoring processes using ps and top
- Managing process priorities using nice and renice
- Sending signals using kill, pkill and killall
- Understanding Linux load averages
- Identifying process states
- Managing performance profiles using TuneD

---

## 1. Process Identification

Commands covered:

```bash
echo $$
ps aux
ps -ef
ps fax
ps -p $$ -o pid,ppid,comm
```

### Key Lessons

- Every process has a unique Process ID (PID).
- PPID identifies the parent process.
- `echo $$` displays the current shell's PID.
- `ps aux` displays detailed process information.
- `ps -ef` displays processes in full-format listing.
- `ps fax` displays process relationships as a tree.

---

## 2. Foreground and Background Job Control

Commands practised:

```bash
sleep 300 &
jobs
fg %1
bg %1
```

Keyboard shortcuts:

- `Ctrl+C` – Interrupt a foreground process.
- `Ctrl+Z` – Suspend a foreground process.
- `bg` – Resume a stopped job in the background.
- `fg` – Bring a job into the foreground.
- `&` – Start a command in the background.

### Key Lessons

Linux allows multiple jobs to run within a shell session. Job control enables processes to be suspended, resumed and moved between the foreground and background.

---

## 3. Managing Process Priorities

Commands practised:

```bash
nice -n 10 sleep 300 &
ps -o pid,ni,comm -p PID
renice -n 15 -p PID
sudo renice -n 0 -p PID
```

Replace `PID` with the actual process ID.

### Nice Values

- `-20` – Highest scheduling priority.
- `0` – Default nice value.
- `19` – Lowest scheduling priority.

### Key Lessons

- `nice` starts a process with a specified nice value.
- `renice` modifies the nice value of an existing process.
- Lower nice values indicate higher scheduling priority.
- Increasing priority generally requires elevated privileges.

---

## 4. Process Signals and Termination

Commands covered:

```bash
kill -l
kill PID
kill -9 PID
pkill -x sleep
killall sleep
```

### Important Signals

| Signal | Number | Purpose |
|---|---|---|
| SIGHUP | 1 | Hangup; some services use it to reload configuration |
| SIGINT | 2 | Interrupt a process |
| SIGKILL | 9 | Forcefully terminate a process |
| SIGTERM | 15 | Request graceful termination |
| SIGCONT | 18 | Continue a stopped process |
| SIGSTOP | 19 | Stop a process |

### Key Lessons

- `kill` sends signals to a process using its PID.
- `pkill` matches processes using name patterns.
- `pkill -x` requires an exact process-name match.
- `killall` targets processes by executable name.
- SIGTERM allows a process to handle termination.
- SIGKILL cannot be caught or ignored.

---

## 5. Monitoring Processes with top

Command:

```bash
top
```

### Interactive Shortcuts

| Key | Action |
|---|---|
| P | Sort processes by CPU usage |
| M | Sort processes by memory usage |
| N | Sort processes by PID |
| T | Sort by accumulated CPU time |
| r | Change a process's nice value |
| k | Send a signal to a process |
| q | Quit top |

### Key Lessons

- `top` provides real-time process monitoring.
- `%CPU` displays CPU utilisation.
- `%MEM` displays memory utilisation.
- Processes can be reniced or terminated directly within top.

---

## 6. Understanding Load Averages

Commands practised:

```bash
uptime
nproc
```

### Actual RHEL 10 VM Results

```text
14:16:19 up 23 min, 2 users, load average: 0.40, 0.38, 0.25
```

```text
2
```

### Interpretation

- 1-minute load average: 0.40
- 5-minute load average: 0.38
- 15-minute load average: 0.25
- Available logical CPUs: 2

The VM was experiencing relatively low system load.

### Key Lessons

- Load average measures tasks running or waiting for CPU, plus tasks in uninterruptible sleep.
- The three values represent 1, 5 and 15-minute averages.
- Load average is not the same as CPU utilisation.
- CPU count helps interpret whether a CPU-bound workload is overloaded.

---

## 7. Linux Process States

Command covered:

```bash
ps -eo pid,stat,comm
```

### Common States

| State | Meaning |
|---|---|
| R | Running or runnable |
| S | Interruptible sleep |
| D | Uninterruptible sleep |
| T | Stopped |
| Z | Zombie |

### Key Lessons

Process states help identify whether a process is executing, waiting, stopped or terminated but not yet reaped.

---

## 8. Managing Performance with TuneD

Commands practised:

```bash
tuned-adm list
tuned-adm active
tuned-adm recommend
sudo tuned-adm profile virtual-guest
```

### Actual RHEL 10 Results

Initially active profile:

```text
Current active profile: balanced
```

Recommended profile:

```text
virtual-guest
```

### Key Lessons

- TuneD manages system performance profiles.
- `tuned-adm list` displays available profiles.
- `tuned-adm active` identifies the current profile.
- `tuned-adm recommend` recommends a profile.
- `virtual-guest` is designed for systems running inside virtual machines.
- `virtual-host` is designed for systems hosting virtual machines.

---

## RHCSA Skills Developed

Through Chapter 10, I developed practical knowledge of:

- Identifying processes and their parent processes
- Managing shell jobs
- Monitoring system resources
- Adjusting process scheduling priorities
- Sending Linux signals
- Understanding load averages
- Identifying process states
- Configuring TuneD performance profiles

## Lab Environment

- Operating System: Red Hat Enterprise Linux 10
- Virtualisation: VMware Workstation
- Shell: Bash
- Certification Preparation: RHCSA EX200
