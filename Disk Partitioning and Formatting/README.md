# 💾 Disk Partitioning and Formatting 

A hands-on guide to mastering Linux storage management, building structural partitions with `fdisk` and `parted`, generating robust filesystems, and locking down persistent system mounts.

## 🎯 Objectives
By the end of this lab, you will be able to:
* **Understand** fundamental disk partitioning workflows and layout topologies in Linux.
* **Practice** volume building utilizing traditional `fdisk` and modern `parted` CLI utilities.
* **Format** unallocated volumes with native `ext4` or `xfs` filesystems using `mkfs`.
* **Configure** persistent system mounts using the critical `/etc/fstab` layout registry.

## 📋 Prerequisites
* A target Linux operating system environment (physical bare-metal instance or virtual machine) with root or `sudo` administrative rights.
* An extra, unallocated practice raw hard disk drive mapped inside your system hardware list (e.g., `/dev/sdb`).
* Basic command line structural familiarity.
* Essential tools pre-installed on the host:
```bash
# For RHEL/CentOS/Fedora systems
sudo dnf install -y util-linux parted e2fsprogs  

# For Ubuntu/Debian systems
sudo apt update && sudo apt install -y util-linux parted e2fsprogs  
```

---

## 🚀 Lab Setup

Before jumping into storage allocation routines, run a clean hardware inventory sweep to reveal block targets:
```bash
lsblk
```
> ⚠️ **Critical Check:** Trace out your designated development storage drive block. The following configuration steps use `/dev/sdb` as a blueprint reference example. Ensure you substitute this path name with your precise local hardware path designation to avoid clearing active host data!

---

## 🛠️ Lab Tasks

### 📐 Task 1: Partitioning a Disk

#### 💻 Subtask 1.1: Using fdisk (MBR/Traditional Layouts)
Initialize the standard interactively driven console workspace tool on your practice disk structure:
```bash
sudo fdisk /dev/sdb
```
##### 🗺️ Quick Shortcut Command Index:
* `n` - Create a clean partition segment
* `p` - Print out the running partition table mapping
* `d` - Wipe out an existing partition allocation
* `t` - Adjust the partition system type identifier index
* `w` - Write out structural adjustments to the disk layout and exit
* `q` - Abort active operations immediately and quit without editing layout tables

##### 🏗️ Constructing a New Partition:
Execute this sequential key entry script choice stream inside the active interface:
1. Press `n` to request a new storage boundary row.
2. Select `p` to design it as a main Primary container layer.
3. Type `1` to define the index map target.
4. Press `Enter` on the first sector field to lock down the starting boundary baseline.
5. Key in `+5G` to build an exact 5 Gigabyte volume track.
6. Press `w` to commit the modifications onto your physical sector tables.

**Expected Outcome:** The terminal prints structural success notes indicating the new block route `/dev/sdb1` is compiled.

#### ⚙️ Subtask 1.2: Using parted (Modern GPT Layouts Alternative)
For solid disk infrastructure allocations scaling far above 2 Terabytes, initialize the modern `parted` engine profile:
```bash
sudo parted /dev/sdb
```
Execute these configuration instructions inside the active prompt:
```text
(parted) mklabel gpt
(parted) mkpart primary ext4 1MiB 5GiB
(parted) print
(parted) quit
```
> 💡 **Troubleshooting Tip:** If the partition manager blocks your commands with an "unrecognized disk label" error warning, make sure you execute the `mklabel gpt` string directive initialization step exactly as demonstrated above before running volume building commands.

---

### 🎛️ Task 2: Formatting Partitions

#### 🛠️ Subtask 2.1: Creating Filesystems
Initialize a standard, highly reliable Linux Extended 4 logging filesystem across the new raw processing boundary space:
```bash
sudo mkfs.ext4 /dev/sdb1
```
*(Alternative Step: If you are setting up an enterprise cloud host environment that prefers high-throughput storage, apply the standard high-performance XFS filesystem syntax format instead)*:
```bash
sudo mkfs.xfs /dev/sdb1
```
Audit the finished storage layer block to query verification layout markers:
```bash
sudo blkid /dev/sdb1
```
**Expected Outcome:** Output strings stream out displaying your raw block layout parameters, showing explicit `TYPE="..."` metadata properties and a unique machine-readable device identifier (`UUID="..."`).

---

### 🔗 Task 3: Mounting and Persistence

#### 📂 Subtask 3.1: Temporary Mount
Form a target point path index inside the unified virtual directory scheme of your root system structure:
```bash
sudo mkdir /mnt/mydata
```
Bind the newly generated partition block explicitly onto your mount target:
```bash
sudo mount /dev/sdb1 /mnt/mydata
```
Confirm the newly available volume size allocations are recognized in memory:
```bash
df -hT /mnt/mydata
```

#### 📌 Subtask 3.2: Persistent Mount
To ensure your hardware allocations remain active after system reboots, map the exact system identifier address string:
```bash
sudo blkid /dev/sdb1
```
Copy down the generated text block inside the `UUID="..."` area fields, then open your global persistent mount allocation tree register inside a standard editor:
```bash
sudo nano /etc/fstab
```
Append your precise persistent storage descriptor map line directly onto the baseline of the system properties file layout structure:
```text
UUID=your-copied-uuid-string-here /mnt/mydata ext4 defaults 0 2
```
Save your edits and exit out of the text editor. Forcefully test your modified initialization file parsing routines before attempting system reboots:
```bash
# Force unmount the live volume first
sudo umount /mnt/mydata

# Instruct the system engine to map all declarations inside the file structure
sudo mount -a
```
If the mount runs cleanly with no errors, execute a full reboot sequence to verify automatic mounting:
```bash
sudo reboot
```
Once the host node returns active online, query the allocation structure maps:
```bash
df -hT /mnt/mydata
```
> 💡 **Troubleshooting Tip:** If your operating system engine locks up or drops into a broken emergency recovery console prompt state during reboot runs because of an `/etc/fstab` configuration typo, enter your administrative root credential loop inside the recovery interface, open your editor to repair or comment out the errant line configuration, and save it.

---

### 🏋️ Advanced Exercise (Optional Swap Volume)
To expand physical system memory capacities using dedicated local disk storage resources:
1. Initialize the disk configuration interface: `sudo fdisk /dev/sdb`
2. Spin up a new partition layer (`n`), and change its internal platform target system profile index map (`t`) over to type code selection value `82` (representing the native Linux Swap space map profile). Write change records out via `w`.
3. Format the raw block surface structure into a standard swap signature layer:
   ```bash
   sudo mkswap /dev/sdb2
   ```
4. Activate the newly formatted block structure straight into live host tracking tables:
   ```bash
   sudo swapon /dev/sdb2
   ```
5. Secure persistent initialization properties by dropping this configuration string into your `/etc/fstab` layout:
   ```text
   /dev/sdb2 none swap sw 0 0
   ```

---

## 🏁 Conclusion
You have mastered the foundational mechanics of Linux physical disk deployment chains:
* Allocated raw processing disk capacities utilizing structural formatting tools (`fdisk` & `parted`).
* Configured file interaction engines (`ext4`/`xfs`) directly across raw partition surfaces.
* Constructed both temporary paths and bulletproof persistent configurations inside `/etc/fstab`.

These foundational file architecture abilities form core requirements when designing persistent local or network storage blocks for cloud engine runtimes, such as container engines operating across cluster environments like OpenShift.

---

## 🧹 Cleanup (Optional Recovery Operations)
To reverse the layout manipulations performed on your environment blocks:
```bash
# Safely clear the tracking layer away from active mount trees
sudo umount /mnt/mydata

# Open /etc/fstab and manually erase your added configuration line statement
sudo nano /etc/fstab

# Erase structural partitions using fdisk or parted tools
sudo parted /dev/sdb rm 1
```

## 📚 Additional Resources
* `man fdisk`
* `man parted`
* `man mkfs`
* `man mount`
* Red Hat Enterprise System Storage Administration Guides
