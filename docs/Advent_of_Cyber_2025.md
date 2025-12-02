# Advent of Cyber 2025 - TryHackMe

## Introduction

Basic Linux commands overview with Windows equivalents:

- `dir /a` (Windows) = `ls -a` (Linux) - List all files including hidden
- `type file` (Windows) = `cat file` (Linux) - Display file contents

---

## Day 1: Linux Commands

### Hidden Files Discovery

We learned about hidden files on Linux and viewing them using `ls -a`, then `cat .file.txt` to find the first flag.

### Log Analysis

**Hint:** Check `/var/log/` and grep inside, let the logs become your guide.

The `/var/log/` folder is where all Linux system logs are stored. We attempted to find information about failed logins using the command `grep "failed login" auth.log`. No results. Then tried `grep "failed password"`. Still nothing, because grep is case-sensitive. Using `grep "Failed password"` returned a list of entries. Found a log of a user trying to connect as socmas:

```
2025-10-13T01:43:48.000724+00:00 tbfc-web01 sshd[1037]: Failed password for socmas from eggbox-196.hopsec.thm port 16212 ssh2
```

### Investigating the Compromised User

Since the user socmas exists, we need to check if they left any malware. Given the attacking machine's name is eggbox, we searched for the word "egg" using `find /home/socmas -name "*egg*"` and found the file `/home/socmas/2025/eggstrike.sh` - possible malware.

**Script analysis:** It reads the wishlist file, dumps it to `/tmp/dump.txt`, deletes it, then replaces it with "easmas.txt".

### System Investigation

To view usernames and password hashes: `cat /etc/shadow`

To run all commands with sudo: `sudo su`

### Command History Analysis

Pressing the "up arrow" shows command history because all commands are saved in `/home/username/.bash_history`. We discovered that someone added an SSH key with `nano .ssh/authorized_keys`, then sent the `data.txt` file to an external server using `curl --data`.
