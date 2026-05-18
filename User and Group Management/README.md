# 👥 User and Group Management in Linux

## 🎯 Objectives
By the end of this lab, you will be able to:
* 👤 Create and manage local system users and groups in Linux.
* ⚙️ Modify critical user attributes, including home directories and default shells.
* 🔗 Assign users to supplementary groups and cleanly remove them when required.

## 📋 Prerequisites
* **Operating System:** A Linux system (e.g., CentOS, Fedora, or Ubuntu).
* **Privileges:** Full root access or administrative `sudo` execution rights.
* **Skills:** Basic familiarity with the Linux command line.

---

## 🛠️ Lab Tasks

### 📂 Task 1: Create New Users and Groups

#### ⚙️ Subtask 1.1: Create a New User
Open a terminal workspace and execute the standard user utility tool to generate a user named `labuser1`:
```bash
sudo useradd labuser1
```
* **Explanation:** By default on many enterprise systems, running `useradd` creates the naked identity framework without automatically forcing home directory configurations.
* **Expected Outcome:** The account structure `labuser1` is created but possesses no active home storage folder path.

To provision a second user explicitly configured alongside an associated home directory path, apply the creation flag:
```bash
sudo useradd -m labuser2
```
* **Explanation:** The `-m` (or `--create-home`) flag instructs the backend to clone `/etc/skel` profile assets straight into a fresh home directory path (`/home/labuser2`).

Audit your newly established identities:
```bash
id labuser1
id labuser2
```
* **Expected Outcome:** Terminal strings dump structural operational metrics specifying the assigned User IDs (UID), Primary Group IDs (GID), and linked group entries.

#### ⚙️ Subtask 1.2: Create a New Group
Establish a fresh group identity called `developers` to consolidate team permissions:
```bash
sudo groupadd developers
```
Verify the group database registration:
```bash
grep developers /etc/group
```
* **Expected Outcome:** The system extracts the matching file entry row directly out of the primary local file database `/etc/group`.

---

### 📂 Task 2: Modify User Information

#### ⚙️ Subtask 2.1: Change User's Home Directory
Reconfigure the primary system path parameters for `labuser1` to map into a new target home directory location:
```bash
sudo usermod -d /home/labuser1_new -m labuser1
```
* **Explanation:** The `-d` parameter defines the absolute destination route, while the `-m` instruction forces the subsystem to physically transfer existing files from the old location over to the new home path.
* **Verification:** Check the high-level system user folders:
  ```bash
  ls /home
  ```
* **Expected Outcome:** The parent tracking directory updates visually, displaying the new `/home/labuser1_new` branch.

#### ⚙️ Subtask 2.2: Change User's Default Shell
Alter the active runtime shell script shell environment path binding for `labuser1`:
```bash
sudo usermod -s /bin/bash labuser1
```
Verify the record state inside the local identity ledger:
```bash
grep labuser1 /etc/passwd
```
* **Expected Outcome:** The final delimiter block segment inside the extracted text configuration string explicitly reflects `/bin/bash`.

---

### 📂 Task 3: Assign Users to Groups and Delete Them

#### ⚙️ Subtask 3.1: Add Users to a Group
Bind your accounts directly into the supplementary security perimeter group `developers`:
```bash
sudo usermod -aG developers labuser1
sudo usermod -aG developers labuser2
```
* **Explanation:** The `-aG` combined switches *append* the user into the targeted secondary group registry. Skipping the `-a` argument can accidentally wipe out all other secondary group memberships.
* **Verification:** Query the user account groupings:
  ```bash
  groups labuser1
  ```
* **Expected Outcome:** The output text array clearly prints `developers` inside the account's active memberships.

#### ⚙️ Subtask 3.2: Remove Users from a Group
Evict the target account from the development group context without disrupting their core user attributes:
```bash
sudo gpasswd -d labuser1 developers
```
Verify the lookup grouping status:
```bash
groups labuser1
```
* **Expected Outcome:** The `developers` group string entry no longer displays inside the text output trace.

#### ⚙️ Subtask 3.3: Delete Users and Groups
Purge the system account profiles and erase their underlying residual physical data footprints:
```bash
sudo userdel -r labuser1
```
* **Explanation:** The `-r` (or `--remove`) argument guarantees that the user’s specific home directory storage tree and mail spools are entirely deleted.

To wipe out the group identity wrapper itself, execute the group removal script:
```bash
sudo groupdel developers
```
Verify complete group deletion:
```bash
grep developers /etc/group
```
* **Expected Outcome:** The terminal window exits cleanly with no output, verifying that the group mapping line is completely missing.

---

## 🏁 Conclusion
By completing this security and system administration laboratory module, you have mastered:
* Provisioning and verifying system identities across Linux distributions using `useradd` and `groupadd`.
* Modifying running account parameters dynamically via `usermod` switches.
* Controlling supplementary group definitions and executing zero-footprint resource cleanups.

These core user-space access control capabilities establish the foundational security concepts required for server administration and align closely with target competencies for Red Hat OpenShift certification tracks.

### 💡 Troubleshooting Tips
* If an account removal instruction fails due to locked files or active system resource warnings, stop background tasks running under that account before retrying:
  ```bash
  sudo pkill -u labuser1
  ```
* If you run into security context blocks or permission denied notifications, ensure your terminal commands are prefixed using the `sudo` administrative execution tool.
