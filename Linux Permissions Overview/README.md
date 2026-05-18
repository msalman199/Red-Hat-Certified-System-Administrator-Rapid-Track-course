# 🔒 Linux Permissions Overview

## 🎯 Objectives
By the end of this lab, you will be able to:
* 🧠 Thoroughly understand Linux file and directory permission structures.
* 📊 Use the `ls -l` command to read and audit standard system permissions.
* 🛠️ Modify access rights using absolute (numeric) and symbolic modes with `chmod`.
* 👥 Change system file ownership and group assignments using `chown` and `chgrp`.

## 📋 Prerequisites
* **Operating System:** A Linux-based system (Ubuntu, CentOS, RHEL, Fedora, etc.).
* **Skills:** Basic familiarity with the Linux command line.
* **Privileges:** A standard non-root user profile configured with `sudo` administrative rights.

---

## 🛠️ Lab Tasks

### 📂 Task 1: Viewing File and Directory Permissions

#### ⚙️ Step 1: Check Current Permissions
Open a terminal workspace and run the long listing command to inspect detailed file metadata:
```bash
ls -l
```
* **Expected Output Example:**
  ```text
  -rw-r--r-- 1 user group 1024 May 18 10:00 file.txt
  drwxr-xr-x 2 user group 4096 May 18 10:01 directory
  ```
* **Explanation:** 
  * `-rw-r--r--` indicates standard file access permissions.
  * `drwxr-xr-x` indicates standard directory access permissions.
  * The absolute first character (`-` for files, `d` for directories, `l` for symlinks) explicitly identifies the file system type.
  * The subsequent nine characters are broken down into three distinct triads representing permissions for the **Owner**, the assigned **Group**, and all **Others**.

#### ⚙️ Step 2: Understanding Permission Notation
* **Permission Breakdown:**
  * `r` = Read permission (view file contents / list directory entries)
  * `w` = Write permission (modify file data / create and delete files within a directory)
  * `x` = Execute permission (run script/binary files / enter and traverse a directory path)
  * `-` = Null placeholder indicating no permission has been granted

* **Example Triad Breakdown:**
  `rwxr-xr--` means:
  * **Owner (u):** Read, Write, and Execute permissions.
  * **Group (g):** Read and Execute permissions.
  * **Others (o):** Read-only permission.

---

### 📂 Task 2: Modifying Permissions with chmod

#### ⚙️ Step 1: Change Permissions Using Absolute (Numeric) Mode
Create a fresh document template for validation testing:
```bash
touch testfile.txt
```
Apply strict numeric permissions to set the file state to `rw-r-----` (Owner: Read/Write, Group: Read, Others: Disallowed):
```bash
chmod 640 testfile.txt
```
Verify your file modification tracking data:
```bash
ls -l testfile.txt
```
* **Expected Output:**
  ```text
  -rw-r----- 1 user group 0 May 18 10:05 testfile.txt
  ```

#### ⚙️ Step 2: Change Permissions Using Symbolic Notation
Dynamically inject execute rights strictly to the primary owner of the asset:
```bash
chmod u+x testfile.txt
```
Explicitly strip away read rights from the public global others class tier:
```bash
chmod o-r testfile.txt
```
Audit the configuration parameters to confirm changes:
```bash
ls -l testfile.txt
```
* **Expected Output:**
  ```text
  -rwxr----- 1 user group 0 May 18 10:05 testfile.txt
  ```

> 💡 **Troubleshooting Tip:** If your terminal system cuts off execution and reports a `Permission denied` block, verify you have administrative right ownership over the asset or elevate the request using `sudo`.

---

### 📂 Task 3: Changing Ownership and Group with chown and chgrp

#### ⚙️ Step 1: Change File Ownership
Spin up an isolated standard account context inside the system architecture ledger if needed for mapping tests:
```bash
sudo useradd testuser
```
Reconfigure the underlying root administrative tracking properties to assign file ownership over to `testuser`:
```bash
sudo chown testuser testfile.txt
```
Verify the identity reassignment results:
```bash
ls -l testfile.txt
```
* **Expected Output:**
  ```text
  -rwxr----- 1 testuser group 0 May 18 10:05 testfile.txt
  ```

#### ⚙️ Step 2: Change Group Ownership
Generate a separate secondary grouping node structure inside the directory mapping layer if needed:
```bash
sudo groupadd testgroup
```
Alter the active group assignment properties tied to the target layout using the group tracking tool:
```bash
sudo chgrp testgroup testfile.txt
```
Verify the combined permission matrix adjustments:
```bash
ls -l testfile.txt
```
* **Expected Output:**
  ```text
  -rwxr----- 1 testuser testgroup 0 May 18 10:05 testfile.txt
  ```

> 💡 **Alternative Optimization:** You can compress both modifications down and execute a combined owner and group adjustment layout using a single `chown` instruction sequence:
> ```bash
> sudo chown testuser:testgroup testfile.txt
> ```

---

## 🏁 Conclusion
By successfully executing this standard access security protocol module, you have mastered:
* Auditing infrastructure file access boundaries through detailed `ls -l` text stream dumps.
* Reconfiguring permission layers utilizing both absolute numeric modes and targeted symbolic notation sets via `chmod`.
* Transitioning ownership profiles and security perimeter wrappers using `chown` and `chgrp`.

These skills form the baseline system defense rules required for managing enterprise computing resources safely and securing underlying directories when configuring application engines like Podman or orchestration fabrics like Red Hat OpenShift.

### 🚀 Next Steps
* Experiment with mixed security parameter permutations to observe access errors.
* Explore the system base creation bitmask values utilizing the `umask` operational tool.
* Apply these identical standard permission boundary concepts inside isolated container file system instances via **Podman**.
