# Linux File System Hierarchy

## 🎯 Objectives
By the end of this lab, you will be able to:
* 🗺️ Understand the standard Linux file system hierarchy and its key directories.
* 🧭 Navigate the Linux file system confidently using command-line tools.
* 🔍 Explore critical system directories and understand their specific administrative purposes.
* 🔗 Distinguish between symbolic links and hard links and understand their roles in the system structure.

## 📋 Prerequisites
* **System:** A Linux-based system (physical machine or virtual machine).
* **Skills:** Basic familiarity with the Linux command line.
* **Privileges:** Terminal access with standard user privileges *(Note: Certain log inspection tasks require `sudo` rights)*.

---

## 🏗️ Lab Setup
No additional software installation is required for the core tasks, as we will use built-in native Linux commands.

---

## 🛠️ Lab Tasks

### 📂 Task 1: Exploring Key Directories

#### ⚙️ Subtask 1.1: Understanding the Root Directory
Open your terminal and list the contents of the primary root directory:
```bash
ls /
```
* **Expected Output:** You will see foundational system paths such as `bin`, `etc`, `home`, `usr`, `var`, and `tmp`.
* **Explanation:** The `/` directory is the absolute root of the entire file system hierarchy. All other directories, partitions, and devices are mounted under it.

#### ⚙️ Subtask 1.2: Exploring /bin and /sbin
Inspect the contents of the binary folders:
```bash
ls /bin
ls /sbin
```
* **🧠 Key Concept:** 
  * `/bin` contains essential user command binaries required for system operation (e.g., `ls`, `cp`, `mv`).
  * `/sbin` contains essential system administration binaries (e.g., `ifconfig`, `fdisk`) typically restricted to privileged users.

#### ⚙️ Subtask 1.3: Examining the /etc Directory
View the configuration directory layout:
```bash
ls /etc
```
Examine a critical system identity file safely using the `less` pager utility:
```bash
less /etc/passwd
```
* **Explanation:** The `/etc` directory acts as the central repository for system-wide configuration files. *Press `q` to exit the less viewer safely.*

#### ⚙️ Subtask 1.4: Working with /home
Jump directly into your personal user workspace:
```bash
cd ~
pwd
```
Generate a blank test document:
```bash
touch testfile.txt
```
* **🧠 Key Concept:** `/home` hosts the personal home directories for standard system accounts, isolating user files, documents, and individual application profile configurations.

#### ⚙️ Subtask 1.5: Exploring /usr
Review the user utilities directory:
```bash
ls /usr
```
Count the total number of executable files bundled inside the primary user binary directory:
```bash
ls /usr/bin | wc -l
```
* **Explanation:** `/usr` stands for *User System Resources*. It houses the vast majority of user utilities, shared libraries, and application binaries. The `wc -l` command counts the line strings to reveal the exact file density.

#### ⚙️ Subtask 1.6: Understanding /var
Review the variable data storage path:
```bash
ls /var
```
Inspect protected system logs using administrative rights:
```bash
sudo ls /var/log
```
* **🧠 Key Concept:** `/var` is reserved for variable data files. This includes data streams that fluctuate constantly during standard operations, such as system event logs, spool files, databases, and temporary caches.

#### ⚙️ Subtask 1.7: Working with /tmp
Switch into the scratch space path and create a temporary tracking block:
```bash
cd /tmp
touch tempfile
```
* **Explanation:** The `/tmp` directory is a universally writable path reserved for volatile temporary data files. Files placed here are frequently flushed automatically during system reboots.

---

### 📂 Task 2: File System Navigation

#### ⚙️ Subtask 2.1: Basic Navigation Commands
Practice checking locations and jumping through directory structures:
```bash
pwd          # Print working directory absolute path
ls -l        # List directory contents with detailed permissions and metadata
cd ..        # Step backward exactly one level into the parent directory
cd           # Instantly return straight to your user home directory (~ context)
```

#### ⚙️ Subtask 2.2: Finding Files
Locate all configuration files ending with a `.conf` suffix inside `/etc`:
```bash
find /etc -name "*.conf"
```
Query the system index database to quickly track down a specific file string:
```bash
locate passwd
```
> 💡 **Troubleshooting Tip:** If your terminal returns a `locate: command not found` or an empty database error, force a manual index refresh before searching:
> ```bash
> sudo updatedb
> ```

---

### 📂 Task 3: Understanding Symbolic Links

#### ⚙️ Subtask 3.1: Identifying Symbolic Links
Filter the `/bin` path directory to isolate file pointer entry marks:
```bash
ls -l /bin | grep '^l'
```
* **Explanation:** Filtering the output with a regular expression checking for `^l` isolates directory records whose attribute lines begin with the letter `l`. This represents a symbolic link pointer.

#### ⚙️ Subtask 3.2: Creating Symbolic Links
Create a fresh mock target document and map a symbolic shortcut (symlink) pointing to it:
```bash
touch original.txt
ln -s original.txt link_to_original
```
Verify the creation of the link pointer layout:
```bash
ls -l link_to_original
```
* **Expected Output:** You should see a clear visual map structure printed: `link_to_original -> original.txt`

#### ⚙️ Subtask 3.3: Understanding Hard Links
Generate a direct hard link attachment mapping to the exact same file blocks:
```bash
ln original.txt hardlink_to_original
```
Compare the low-level file system index numbers (inodes) of all three files:
```bash
ls -i original.txt hardlink_to_original link_to_original
```
* **🧠 Key Concept:** Hard links are directory entries that map directly to the exact same underlying disk space allocation (**inode number**). Symbolic links (`-s`) function as simple text pointers containing the string path to another file name and carry a unique inode number of their own.

---

## 🏁 Conclusion
By completing this structural file system exploration lab, you have mastered:
* Mapping directory purposes across standard Linux paths (`/bin`, `/etc`, `/var`).
* Filtering, tracking, and indexing files across the storage system tree.
* Generating and analyzing the fundamental differences separating symbolic links from hard links.

Acquiring a deep understanding of the standard Linux file system structure is essential for server administration, infrastructure engineering, and troubleshooting software deployments inside enterprise container fabrics like Red Hat OpenShift.

### 💡 Troubleshooting Tips
* **Permission Denied Errors:** Prefix your diagnostic and tracking scripts using the administrative override command: `sudo`.
* **Command Not Found Errors:** Double-check your path arrays or manually verify that the execution binaries are correctly located inside `/bin`, `/usr/bin`, or `/sbin`.
* **Reading System Help Documentation:** Read the official kernel documentation for any unfamiliar tool using the built-in manual pager system: `man <command_name>`.
* **Visualizing Filesystem Trees:** You can render a visual tree structure diagram by installing the open-source directory listing utility:
  ```bash
  sudo apt install tree     # Debian / Ubuntu
  sudo dnf install tree     # RHEL / CentOS / Fedora
  ```

### 🚀 Next Steps
* Move into kernel space environments by inspecting pseudo-filesystems like `/proc` and `/sys`.
* Learn how to modify resource restrictions and ownership permissions via `chmod` and `chown`.
* Practice parsing unstructured log file blocks using deep text-filtering scripts like `grep`, `awk`, and `sed`.
