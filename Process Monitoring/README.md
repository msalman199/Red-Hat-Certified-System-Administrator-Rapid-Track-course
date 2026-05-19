# 📊 Process Monitoring Lab

A hands-on guide to tracking system performance, identifying resource-intensive bottlenecks, and optimizing Linux system performance using `top`, `ps`, and `htop`.

## 🎯 Objectives
By the end of this lab, you will be able to:
* **Monitor** real-time system processes with multiple CLI tools.
* **Identify** resource-intensive processes causing performance bottlenecks.
* **Optimize** system performance by safely terminating or prioritizing processes.

## 📋 Prerequisites
* A Linux-based system (Ubuntu, Debian, CentOS, RHEL, or Fedora).
* Basic familiarity with the command line interface.
* Administrative (`sudo`) privileges for process management tasks.

---

## 🚀 Lab Setup

### ⚙️ Step 1: Install Required Tools
Ensure `htop` is installed on your specific Linux distribution:

```bash
# For Debian/Ubuntu
sudo apt update && sudo apt install -y htop  

# For CentOS/RHEL
sudo yum install -y epel-release && sudo yum install -y htop  

# For Fedora
sudo dnf install -y htop  
```
**Expected Outcome:** `htop` is successfully installed and ready for global execution.
> 💡 **Troubleshooting Tip:** If your package manager fails, check your internet connectivity or verify repository configuration files.

---

## 🛠️ Lab Tasks

### 🔍 Task 1: View Running Processes

#### 💻 Step 1.1: Using top
Run the standard dynamic real-time utility:
```bash
top
```
* **Key Observations:** Look at `%CPU` for processing bottlenecks, `%MEM` for RAM hogs, and note the `PID` (Process ID).
* **Exit:** Press `q`.

**Expected Outcome:** A dynamic, self-refreshing view of running processes sorted automatically by CPU usage.

#### 📄 Step 1.2: Using ps
List all active system processes with detailed technical information:
```bash
ps aux
```
* **a** = Show processes for all users.
* **u** = Display user-oriented descriptive format.
* **x** = Include processes not attached to a terminal interface.

**Expected Outcome:** A comprehensive static snapshot listing resource usage for every active process.

#### 🎨 Step 1.3: Using htop
Launch the enhanced interactive tool:
```bash
htop
```
* **Key Features:** Color-coded usage meters, Tree View (`F5`) for parent-child paths, and instant sorting (`F6`).
* **Exit:** Press `F10` or `q`.

**Expected Outcome:** An interactive, user-friendly dashboard for visually tracking performance.

---

### 🚨 Task 2: Identify Resource Hogs and Bottlenecks

#### 🏎️ Step 2.1: Find High CPU Usage Processes
Filter and isolate the top 5 CPU-consuming utilities:
```bash
ps aux --sort=-%cpu | head -n 5
```
**Expected Outcome:** Clean list of the top 5 processes demanding the most processing power.

#### 🧠 Step 2.2: Find High Memory Usage Processes
Filter and isolate the top 5 RAM-consuming utilities:
```bash
ps aux --sort=-%mem | head -n 5
```
**Expected Outcome:** Clean list of the top 5 processes consuming the most system memory.

#### ⚖️ Step 2.3: Check System Load Average
Check the overall operational pressure on the system processor:
```bash
uptime
```
* **Key Metric:** Load averages are displayed for 1, 5, and 15-minute intervals. Numbers higher than your total CPU core count indicate performance saturation.

**Expected Outcome:** Current system runtime and exact load averages printed to the screen.

---

### ⚡ Task 3: Optimize System Performance

#### ❌ Step 3.1: Kill a Process
Terminate a non-critical or frozen process using its unique identifier:
```bash
# Graceful execution (Sends SIGTERM to allow clean file saving)
kill <PID>  

# Forced termination (Sends SIGKILL to stop the process immediately)
kill -9 <PID>  
```
> ⚠️ **Warning:** Using `kill -9` forces an immediate shutdown without cleanups. Use it cautiously to avoid data corruption.

#### 📈 Step 3.2: Renice a Process
Change the scheduling priority of an active, resource-heavy execution:
```bash
sudo renice -n 10 -p <PID>
```
* **Niceness Scale:** Ranges from `-20` (Highest priority, takes more CPU) to `19` (Lowest priority, yields to other tasks).

**Expected Outcome:** The selected process priority gets successfully shifted to match system needs.

---

## 🏁 Conclusion
You have mastered the baseline mechanics of Linux performance analysis:
* Monitored system metrics using `top`, `ps`, and `htop`.
* Pinpointed processing and memory usage spikes.
* Tuned system performance by terminating or re-prioritizing rogue workflows.

### 🗺️ Next Steps
* **Automation:** Write automated Bash monitoring scripts utilizing `cron` for long-term telemetry logs.
* **Advanced Monitoring:** Explore deep observation frameworks like `glances` or `nmon`.

### ✅ Verification Check
Verify the completion of your lab run:
```bash
echo "Lab 13 - Process Monitoring completed successfully!"
```

---

## 🩺 Troubleshooting Appendix


| Problem | Cause | Resolution |
| :--- | :--- | :--- |
| **Permission Denied** | Missing root execution rights. | Prepend `sudo` to your `kill` or `renice` instructions. |
| **Process Not Found** | The PID is missing or typed incorrectly. | Locate the actual runtime ID using `ps aux \| grep <process_name>`. |
| **High Load Persists** | Hidden background hardware or core system errors. | Audit deeper hardware logs via `dmesg` or `journalctl -xe`. |
