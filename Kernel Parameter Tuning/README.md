# ⚙️ Kernel Parameter Tuning 

A hands-on guide to analyzing, modifying, and optimizing Linux kernel runtime behaviors using `sysctl`, targeting specific networking enhancements and memory management profiles.

## 🎯 Objectives
By the end of this lab, you will be able to:
* **View** and dynamically alter active Linux kernel runtime variables.
* **Tune** underlying network thresholds and memory paging allocation profiles.
* **Test** and validate stability variations under synthetic operational workloads.

## 📋 Prerequisites
* A Linux-based operating system (CentOS, RHEL, Ubuntu, or Debian).
* Root administrative access or explicit `sudo` privileges.
* Basic familiarity with the Linux command line interface.

---

## 🚀 Lab Tasks

### 🔍 Task 1: View and Modify Kernel Parameters with sysctl

#### 💻 Subtask 1.1: View Current Kernel Parameters
Dump all active kernel configuration trees currently loaded into system memory:
```bash
sudo sysctl -a
```
**Expected Outcome:** A comprehensive list of all tunable parameters is output to the terminal.

Query a single specific configuration item, such as the system virtual memory swappiness index:
```bash
sudo sysctl vm.swappiness
```
**Expected Outcome:** Returns the active value (typically defaults to `60`).

#### ⏱️ Subtask 1.2: Temporarily Modify a Kernel Parameter
Alter runtime variables dynamically in memory (this setting will reset back to default upon system reboot):
```bash
sudo sysctl -w vm.swappiness=10
```
> ℹ️ **Explanation:** Lowering this value to `10` tells the kernel to avoid swapping active processes from RAM to disk storage unless absolutely necessary.

Verify that the memory state updated successfully:
```bash
sudo sysctl vm.swappiness
```
**Expected Outcome:** The screen explicitly displays `vm.swappiness = 10`.

#### 💾 Subtask 1.3: Permanently Modify a Kernel Parameter
Open the global system runtime variables file inside a text editor:
```bash
sudo nano /etc/sysctl.conf
```
Append your custom tuning definitions directly to the bottom of the configuration file:
```text
vm.swappiness = 10
```
Save and exit the text editor, then force the kernel to parse and commit your persistent updates immediately:
```bash
sudo sysctl -p
```
**Expected Outcome:** The configuration path is printed back, indicating it will now survive system reboots.

---

### 🛠️ Task 2: Tune Parameters for Networking and Memory Management

#### 🌐 Subtask 2.1: Optimize Network Performance
Scale up the global file descriptor limits to prevent socket drops under heavy network loads:
```bash
sudo sysctl -w fs.file-max=100000
```
Activate TCP Fast Open capabilities to speed up consecutive connection setups:
```bash
sudo sysctl -w net.ipv4.tcp_fastopen=3
```
Confirm the active values have stepped up:
```bash
sudo sysctl fs.file-max net.ipv4.tcp_fastopen
```

#### 🧠 Subtask 2.2: Adjust Memory Management Settings
Configure the virtual memory sub-allocation subsystem to tolerate memory overcommitments:
```bash
sudo sysctl -w vm.overcommit_memory=1
```
> ℹ️ **Explanation:** A value of `1` instructs the kernel to always allow memory allocations, which is ideal for performance-heavy database and container environments.

Reclaim standard hugepage limits to match application processing definitions:
```bash
sudo sysctl -w vm.nr_hugepages=0
```
Confirm the adjustments:
```bash
sudo sysctl vm.overcommit_memory vm.nr_hugepages
```

---

### 🧪 Task 3: Test the Effect of Kernel Changes

#### 📊 Subtask 3.1: Monitor System Performance
Verify baseline memory spaces before running workloads:
```bash
free -h
```
Review active connection states and socket telemetry statistics:
```bash
ss -s
```

#### 🏋️ Subtask 3.2: Simulate Workload and Observe Changes
Install a dedicated system stress generator to simulate production compute loads:
```bash
# For Debian/Ubuntu systems
sudo apt install -y stress-ng  

# For RHEL/CentOS systems
sudo yum install -y stress-ng  
```
Launch a 60-second heavy operational memory test utilizing multiple hardware threads:
```bash
stress-ng --vm 2 --vm-bytes 2G --timeout 60s
```
While the stress simulation is active, open a second terminal panel to track behavior real-time:
```bash
top
```
**Expected Outcome:** The system monitors show lower swap interaction and smoother host stabilization under memory stress due to your `vm.swappiness` changes.

Examine raw maximum throughput speeds using connection load testers:
```bash
iperf3 -c <server-ip>
```
**Expected Outcome:** Shows increased network throughput scaling if the underlying infrastructure supports TCP Fast Open optimizations.

---

## 🏁 Conclusion
You have successfully completed the following milestones:
* Audited and evaluated running parameters using the `sysctl` workspace.
* Temporarily and permanently adjusted critical parameters to build optimized baseline layers.
* Simulated workload pressures to verify performance adjustments.

---

## 🩺 Troubleshooting Appendix

* **Settings Not Applying:** Carefully double-check your syntax entries inside `/etc/sysctl.conf`. Typos or trailing spaces will cause parsing errors.
* **Kernel Level Errors:** Run `dmesg` or check your system logging loops (`journalctl -xe`) to track syntax warnings or invalid parameter assignment flags thrown by the system.
* **Emergency Rollbacks:** To wipe out custom parameters, simply delete your edited line entries from `/etc/sysctl.conf` and re-execute a clean reload operation using `sudo sysctl -p`.

### 🗺️ Next Steps
* **Deep Tuning:** Investigate more advanced network and memory tuning variables like `net.core.somaxconn` or `vm.dirty_ratio`.
* **Automation:** Package your custom configuration strings into configuration management scripts like **Ansible playbooks** or automation scripts for easy, reproducible deployments across large-scale servers.
