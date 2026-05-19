# 🌐 Configuring and Mounting NFS Lab

A hands-on guide to establishing a Network File System (NFS) cluster architecture. Learn to implement centralized storage pools, export network pathways securely from a master server node, and construct persistent auto-mounting file frameworks on remote client machines.

## 🎯 Objectives
By the end of this lab, you will be able to:
* **Configure and mount** Network File System (NFS) shares for centralized data access.
* **Set up** an NFS server engine and export directories with precise client access controls.
* **Configure** a remote NFS client to attach network-shared directories smoothly.
* **Test** file locking and secure persistent mounting mappings across system reboots.

## 📋 Prerequisites
* Two distinct Linux network hosts (one designated as the Storage Server, one as the Client).
* Root execution boundaries or explicit `sudo` administrative rights on both environments.
* Inter-node network connectivity (validate ahead of time using `ping <target_ip>`).

---

## 🚀 Task 1: Set Up NFS Server and Export Directories

### ⚙️ Subtask 1.1: Install NFS Server Packages
Log into your designated **NFS Server** machine and deploy the necessary platform infrastructure packages:

```bash
# For RHEL/CentOS/Fedora systems:
sudo dnf install -y nfs-utils

# For Debian/Ubuntu systems:
sudo apt update && sudo apt install -y nfs-kernel-server
```
Enable the structural transport engines to boot up automatically with the kernel system initialization:
```bash
sudo systemctl enable --now nfs-server
```
**Expected Outcome:** The NFS daemon transitions into a live state. Verify this profile via `systemctl status nfs-server` (look for the `active (running)` status marker).

### 📁 Subtask 1.2: Create and Export a Shared Directory
Construct a baseline raw folder layer directly inside the root file paths to serve as your central file pool:
```bash
sudo mkdir /shared
sudo chown nobody:nobody /shared  # Bind security mapping profiles for anonymous users
sudo chmod 777 /shared            # Open temporary permissions matrix for testing phases
```
Open the core network export directory routing table index file inside your text editor:
```bash
sudo nano /etc/exports
```
Append your precise share distribution string rules onto a clean line block (make sure to substitute `client_ip` with your genuine target client node IP address or target subnet range):
```text
/shared client_ip(rw,sync,no_root_squash)
```
* **rw:** Provides dual read and write operation capabilities to the attached clients.
* **sync:** Forces instant synchronous operations, writing modifications to disk files before acknowledging commands.
* **no_root_squash:** Allows the root user account on the client side to manage system files with original root rights on the server storage layer.

Commit your newly added configuration boundaries into active server memory loops immediately:
```bash
sudo exportfs -arv
```
**Expected Outcome:** Validate current active rules utilizing `sudo exportfs -v`. The `/shared` resource folder block path should print out properly alongside its matching permission flags.
> 💡 **Troubleshooting Tip:** If your structural configurations fail to propagate or get blocked, trigger an explicit service reset via `sudo systemctl restart nfs-server`.

---

## 🚀 Task 2: Configure NFS Client

### 📦 Subtask 2.1: Install NFS Client Packages
Log over into your separate **Client** system environment and stand up the requisite network client utilities:
```bash
# For RHEL/CentOS/Fedora clients:
sudo dnf install -y nfs-utils

# For Debian/Ubuntu clients:
sudo apt update && sudo apt install -y nfs-common
```

### 📂 Subtask 2.2: Mount the NFS Share
Generate a dedicated empty access path directory inside your client's local virtual tree layout:
```bash
sudo mkdir -p /mnt/nfs
```
Execute a manual network filesystem mount instruction pointing directly toward your remote storage node (substitute `server_ip` with the authentic network address of your NFS Server):
```bash
sudo mount -t nfs server_ip:/shared /mnt/nfs
```
Audit the active storage maps to verify that the cross-host attachment finished cleanly:
```bash
df -hT | grep nfs
```
**Expected Outcome:** Your remote storage space block displays clearly in the printed console tables (e.g., `server_ip:/shared` bound cleanly over `/mnt/nfs`).

### 📝 Subtask 2.3: Test Read/Write Access
Create a dummy tracking document inside the network path directory to confirm storage write operations:
```bash
sudo touch /mnt/nfs/testfile
ls -l /mnt/nfs
```
**Expected Outcome:** The file `testfile` displays properly inside the catalog list, indicating data is writing to the server.

---

## 🚀 Task 3: Persistent NFS Mounts

### 📌 Subtask 3.1: Configure /etc/fstab for Auto-Mounting
To avoid manually running mount strings whenever the client computer drops power, drop a permanent configuration statement into the storage register layout file:
```bash
sudo nano /etc/fstab
```
Append your network path configuration statement exactly onto the bottom layer of the file configuration strings (remember to swap in your authentic `server_ip` identifier):
```text
server_ip:/shared  /mnt/nfs  nfs  defaults  0  0
```
Unmount your temporary mount to run an authentic verification run, then force the engine to parse the revised initialization settings file:
```bash
sudo umount /mnt/nfs
sudo mount -a
```
**Expected Outcome:** The command processes silently with zero error returns. Running `df -hT` confirms that the network-attached storage pool is active again.

### 🔄 Subtask 3.2: Reboot and Verify Persistence
Trigger a clean power cycle on the active client system node to guarantee that your modifications survive reboots:
```bash
sudo reboot
```
Once the user workspace returns back to online status, run an automated terminal telemetry check:
```bash
df -hT | grep nfs
```
**Expected Outcome:** The client environment recognizes and re-mounts the remote `/shared` folder path.

---

## 🏁 Conclusion
You have successfully deployed a functional Network File System matrix architecture:
* Staged a fully operational storage service server and built flexible file extraction lists inside `/etc/exports`.
* Initialized client instances, verified cross-host transport configurations, and confirmed read/write functionality.
* Engineered persistent storage mounting parameters using the system layout options inside `/etc/fstab`.

---

## 💡 Key Concepts
* **NFS:** Network File System framework designed to serve uniform centralized file structures over standard local networks.
* **Exports File:** The absolute server-side configuration baseline mapping which storage zones can be accessed by specified network node endpoints.
* **Fstab Registry:** The core client-side mounting properties file indicating which external storage locations deploy during early hardware boot sequences.

---

## 🩺 Troubleshooting Tips

* **"Access Denied" Errors:** Double-check client IP path matching alignments inside `/etc/exports` on your server. Ensure you reload configuration maps via `sudo exportfs -arv`.
* **Mount Failures:** Verify physical link visibility between devices using basic network tests (`ping server_ip`). Check that firewall rules allow traffic through on both ends.
* **Slow Performance Bottlenecks:** For low-latency production needs where immediate safety cycles can be balanced against transport velocities, try swapping out `sync` for the highly asynchronous alternative rule string `async` inside your `/etc/exports` file parameters.

### 🗺️ Next Steps
* Secure your data environments by implementing enterprise-grade **NFS Security Layer Extensions** (e.g., configuring Kerberos cryptographic authentication routines or managing targeted port firewall isolation rules).
