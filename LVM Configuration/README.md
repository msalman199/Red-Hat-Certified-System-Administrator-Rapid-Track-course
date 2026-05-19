# 🎛️ LVM Configuration

A hands-on guide to mastering Linux Logical Volume Manager (LVM) abstractions. Learn to pool raw physical storage devices, allocate dynamic software boundaries, scale filesystems live, and manage point-in-time storage recovery states.

## 🎯 Objectives
By the end of this lab, you will be able to:
* **Understand** and implement flexible abstraction layers using Logical Volume Manager.
* **Build** and link Physical Volumes (PVs), Volume Groups (VGs), and Logical Volumes (LVs).
* **Scale** file footprints dynamically via live volume growth and safe reduction trees.

## 📋 Prerequisites
* A Linux host environment (RHEL, CentOS, or Fedora recommended).
* Root execution rights or explicit `sudo` administrative privileges.
* Available unallocated raw block tracking devices (physical or virtual hard drives).
* Baseline command line familiarity and foundational storage concepts.

---

## 🚀 Lab Setup

Before mapping logical layers, poll the system block nodes to identify eligible drive targets:
```bash
lsblk
sudo fdisk -l
```
> 🗄️ **Target Drive:** Identify a raw target disk profile (e.g., `/dev/sdb`) to use throughout this blueprint. Ensure that your chosen target block device is clear of active system operational partitions.

---

## 🛠️ Lab Tasks

### 📦 Task 1: Create Physical Volumes (PVs) and Volume Groups (VGs)

#### ⚙️ Subtask 1.1: Create Physical Volumes
Convert a raw unallocated disk target into an integrated LVM physical storage node:
```bash
sudo pvcreate /dev/sdb
```
Verify the newly injected structural initialization details:
```bash
sudo pvdisplay
```
**Expected Output:** Detailed metadata parameters detailing total disk size, Physical Extent (PE) allocation boundaries, and completely unassigned free space metrics.
> 💡 **Troubleshooting Tip:** If the system alerts you that the target device is currently in use, clear active maps using `umount /dev/sdb`. If old block residue remains on the surface, clean the partition tables completely via `sudo wipefs -a /dev/sdb`.

#### 📂 Subtask 1.2: Create Volume Group
Group your individual active Physical Volumes into a combined virtual hardware pool named `vg01`:
```bash
sudo vgcreate vg01 /dev/sdb
```
Confirm the unified block space capacity profile:
```bash
sudo vgdisplay vg01
```
**Expected Output:** Complete configuration profile listing the active VG identity, physical storage extent boundaries, combined capacities, and free slice fields.

---

### 🎛️ Task 2: Create Logical Volumes (LVs) and Format Them

#### ✂️ Subtask 2.1: Create Logical Volume
Carve out a clean 5 Gigabyte logical allocation slice named `lv01` directly from the `vg01` storage group:
```bash
sudo lvcreate -L 5G -n lv01 vg01
```
Verify the resulting sub-allocation mapping layer:
```bash
sudo lvdisplay /dev/vg01/lv01
```
**Expected Output:** Structural storage maps outlining device node system pathways, size constraints, and block identifiers.

#### 🛠️ Subtask 2.2: Format and Mount LV
Generate a standard, resilient Extended 4 journaling file layout format engine right across your virtual allocation space:
```bash
sudo mkfs.ext4 /dev/vg01/lv01
```
Construct an empty point directory inside your root layout paths and secure the running block storage mount:
```bash
sudo mkdir /mnt/lv01
sudo mount /dev/vg01/lv01 /mnt/lv01
```
Validate that the live file access capacities sync up appropriately:
```bash
df -h /mnt/lv01
```
**Expected Output:** Active workspace node layout confirmation showing a clean 5GB file target allocation area.
> 💡 **Troubleshooting Tip:** If your mount commands throw initialization exceptions, sweep system messages using `dmesg` to check for underlying disk formatting corruption signatures.

---

### ⚡ Task 3: Extend and Shrink LVs

#### 📈 Subtask 3.1: Extend a Logical Volume
Scale up your logical volume dynamically in real time by injecting an extra 2 Gigabytes of capacity:
```bash
sudo lvextend -L +2G /dev/vg01/lv01
```
Expand your running `ext4` filesystem interface layer to consume the newly allocated physical boundaries without interrupting services:
```bash
sudo resize2fs /dev/vg01/lv01
```
Verify the updated target scale thresholds:
```bash
df -h /mnt/lv01
```
**Expected Output:** Storage capacities dynamically update to show an approximate ~7GB active file operational range.

#### 📉 Subtask 3.2: Shrink a Logical Volume
> ⚠️ **Warning:** Filesystem containment reduction requires volume unmounting and introduces structural stability data corruption risks if misconfigured. Back up critical data files before proceeding!

Isolate the data system pathways away from running apps, then perform a mandatory sector file structure scan:
```bash
sudo umount /mnt/lv01
sudo e2fsck -f /dev/vg01/lv01
```
Downscale the physical size of the internal file system framework structures down to a target baseline size of 4 Gigabytes first:
```bash
sudo resize2fs /dev/vg01/lv01 4G
```
Reduce the underlying logical allocation boundaries to match your downscaled filesystem size exactly:
```bash
sudo lvreduce -L 4G /dev/vg01/lv01
```
Remount the revised virtual path matrix block and verify the structural shrink:
```bash
sudo mount /dev/vg01/lv01 /mnt/lv01
df -h /mnt/lv01
```
**Expected Output:** The resulting storage footprint cleanly scales down to show a restricted ~4GB allocation target boundary.

---

### 📸 Advanced Operations (Optional Snapshots)
To capture a point-in-time recovery backup frame of your logical environment setup:
```bash
sudo lvcreate -s -n lv01_snap -L 1G /dev/vg01/lv01
```
Examine file integrity signatures inside the frozen capture state by pinning the snapshot volume onto a temporary target directory:
```bash
sudo mkdir /mnt/snap
sudo mount /dev/vg01/lv01_snap /mnt/snap
```

---

## 🏁 Conclusion
You have mastered modern enterprise Linux storage layout mechanics:
* Created structural volume groupings out of raw physical block devices.
* Set up customizable filesystems inside logical blocks.
* Managed non-destructive growth modifications and performed downscales safely.
* Implemented quick system data restoration capturing frameworks via snapshots.

---

## 🩺 Troubleshooting Tips

* **Undetected Volume Group Structures:** Force a low-level hardware re-scan across disk paths via `vgscan`, and activate layout parameters instantly using `vgchange -ay`.
* **Subsystem Warnings:** Parse core system loop histories inside `/var/log/messages` or audit logs using `journalctl -xe` to catch layout warnings.
* **Thin Provisioning:** For building shared enterprise physical allocation groups, deploy thin execution blocks using the `-T` flag option string instead of using `-L`.

---

## 🧹 Cleanup (Optional Wipe Routines)
To drop your staging changes and return hardware tracks to clean unassigned space definitions:
```bash
sudo umount /mnt/lv01
sudo lvremove /dev/vg01/lv01 -y
sudo vgremove vg01
sudo pvremove /dev/sdb
```

## 📚 Additional Resources
* `man lvm` - Deep system LVM usage references
* Red Hat Storage Administration Guide manuals (RHCSA/RHCE targets)
* Linux Documentation Project architectural summaries: `tldp.org/HOWTO/LVM-HOWTO`
