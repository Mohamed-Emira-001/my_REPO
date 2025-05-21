
# Linux Essentials Guide

A concise, well-structured reference for key Linux concepts: file systems, user management, permissions, process control, LVM, and more.

---

## Linux File System Hierarchy

| Directory | Purpose |
|----------|---------|
| `/` | Root directory — the starting point of the entire Linux file system. |
| `/proc` | Virtual filesystem providing process and system information. |
| `/etc` | Contains system-wide configuration files (editable text files). |
| `/bin` | Essential command binaries required for system boot and operation. |
| `/sbin` | System binaries — critical commands for system administration. |
| `/home` | User home directories (`~` refers to the current user's home). |

---

## 👤 Users and Groups

- `/etc/shadow` stores encrypted user passwords.
- `/etc/passwd` format:
  ```
  username:password:UID:GID:Real-Name:Home-dir:Default-Shell
  ```
- UID breakdown:
  - `0` — root (superuser)
  - `1-200` — static system users
  - `201-999` — dynamic system users

---

##  Hard vs Soft Links

| Type | Command | Characteristics |
|------|---------|------------------|
| Hard Link | `ln file linkname` | Same inode, same file system, can't link directories |
| Soft Link | `ln -s target linkname` | Points to file, can span filesystems, can link to dirs |

---

##Variable Expansion

```bash
USERNAME=operator
echo $USERNAME
# Output: operator
```

---

## `su` vs `su -`

| Command | Env Variables | Working Dir | Shell Profile |
|---------|---------------|-------------|----------------|
| `su` | Keeps current | Stays the same | No |
| `su -` | Loads target | Changes to home | Yes |

---

## Redirecting & Piping

- **File Descriptors**: stdin (0), stdout (1), stderr (2)
- **Redirect Examples**:
  ```bash
  command > file.txt        # stdout to file
  command 2> error.log      # stderr to file
  command &> all.log        # stdout + stderr
  ```
- **Piping**: Output of one command becomes input of another
  ```bash
  ls -l /usr/bin | less
  ls -t | head -n 10 > /tmp/latest.txt
  ```
- **Null Device**: Use `/dev/null` to discard output

---

## `sudoers` File

### `%admin ALL=(ALL) ALL` Explanation

- `%admin` → Applies to `admin` group
- `ALL=` → Can run as any user
- `(ALL)` → Can run as any group
- `ALL` → Can execute any command

---

## File Permissions

- `ls -ld` — shows directory details
- File types:
  - `-` regular file
  - `d` directory
  - `l` symbolic link
- Change permissions:
  ```bash
  chmod -R g+rwX folder
  chown :groupname file
  ```

---

## Process Management

| Code | State | Description |
|------|-------|-------------|
| R | Running | Currently executing |
| S | Sleeping | Waits for event (interruptible) |
| D | Disk Sleep | Waits for I/O (uninterruptible) |
| T | Stopped | Stopped by signal |
| Z | Zombie | Terminated but not reaped |
| X | Dead | Not in use (rare) |

- Background job: `command &`
- Systemd commands:
  ```bash
  systemctl status service
  systemctl list-dependencies sshd.service
  ```

---

## `apt update` - GET vs HIT

| Status | Meaning |
|--------|---------|
| `GET` | Downloading fresh metadata |
| `HIT` | Already up-to-date in cache |

---

## Physical vs Logical Volumes

| Feature | Physical Volume (PV) | Logical Volume (LV) |
|--------|------------------------|----------------------|
| Created with | `pvcreate` | `lvcreate` |
| Can be mounted | NO | YES |
| Resizable | NO | YES |
| Lives in | Disk partition | Volume Group |
| Used for | Pooling storage | Storing files |

### Volume Flow:

```
[Physical Volume] → [Volume Group] → [Logical Volume] → [Filesystem & Mounting]
```

- Volume Group is a storage pool.
- Add more PVs to extend VG size.
- Filesystem must be resized manually.
---
### Difference Between `wget` and `curl`

| Feature            | `wget`     | `curl`                     |
| ------------------ | ---------- | -------------------------- |
| File download      |  Yes      |  Yes                      |
| Website download   |  Yes      |  No                       |
| API interaction    |  No       |  Yes                      |
| Resuming downloads |  Yes      |  Yes                      |
| Recursive download |  Yes      |  No                       |
| Simplicity         |  Easier   |  More options (powerful) |
| Scripting & APIs   |  Limited |  Best choice              |


-----------------------------------------------------------------------------------------------------------------------
- Repository Management
# The Primary Repository Configuration file Found in:
						
```
sudo nano /etc/apt/source.list
```
# To Install Specific Package version
```
sudo apt install <package-name>=version
```
# Package Information Retrieval
```
sudo apt show <package-name>
```
---

### Steps to Download a Package manually
```
- find a link to download package file <always ended with (.deb)>
wget <link>
- Install the package manually by dpkg (Because it deals with .deb files)
sudo dpkg -i <.deb file>
- Fix Dependencies if needed
sudo apt-get install -f
```
