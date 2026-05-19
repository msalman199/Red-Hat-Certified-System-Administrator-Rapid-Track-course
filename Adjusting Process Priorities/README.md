# ⚡ Adjusting Process Priorities Lab

A hands-on guide to mastering Linux process scheduling, launching prioritized applications using `nice`, altering active runtimes with `renice`, and monitoring resource balancing metrics.

## 🎯 Objectives
By the end of this lab, you will be able to:
* **Understand** fundamental Linux process scheduling and priority concepts.
* **Launch** workflows with custom execution weights using `nice`.
* **Modify** existing runtimes dynamically using `renice`.
* **Observe** performance changes and prevent resource starvation via metric logs.

## 📋 Prerequisites
* A Linux-based system (Fedora, RHEL, or CentOS recommended).
* Basic command line proficiency.
* Root or `sudo` administrative privileges.

---

## 🚀 Setup Requirements

### ⚙️ Install Monitoring Tools
Ensure your tracking interfaces are ready for profiling:
```bash
sudo dnf install -y htop glances
```
Verify the installations:
```bash
which htop glances
```

---

## 🛠️ Lab Tasks

### 🔍 Task 1: Starting Processes with Different Priorities Using nice

#### 🧠 Subtask 1.1: Understanding Nice Values
* Values range from `-20` (Highest priority, takes more CPU) to `19` (Lowest priority, yields to other tasks).
* The baseline default priority is `0`.
* Regular unprivileged users can only lower priority (increase the nice number).
* `root` or `sudo` administrative access is required to increase priority (negative numbers).

#### 📉 Subtask 1.2: Launching a Low-Priority Process
Start a safe background processor at the lowest possible system tier:
```bash
nice -n 19 sha1sum /dev/zero &
```
**Expected Output:**
```text
[1] 12345  # Note down your unique background Process ID (PID)
```

#### 🏎️ Subtask 1.3: Launching a High-Priority Process
Launch a competing processing stream at the maximum scheduling preference:
```bash
sudo nice -n -20 sha1sum /dev/zero &
```

#### ✅ Subtask 1.4: Verify Priorities
Inspect the scheduling tables to confirm priority changes:
```bash
ps -l -p $(pgrep sha1sum)
```
**Expected Output:**
```text
F S   UID     PID    PPID  C PRI  NI ADDR SZ WCHAN  TTY        TIME CMD
4 R     0   12346    4567 99  60 -20 -  1234 -      pts/0      0:10 sha1sum /dev/zero
0 R  1000   12345    4567 99  80  19 -  1234 -      pts/0      0:10 sha1sum /dev/zero
```
> 💡 **Troubleshooting Tip:** If you encounter a "permission denied" error when testing negative levels, check your syntax to make sure `sudo` prepends the `nice` statement.

---

### 🛠️ Task 2: Modifying Running Process Priorities with renice

#### 🎯 Subtask 2.1: Identify Target Process
Locate the background worker tracking IDs:
```bash
pgrep sha1sum
```

#### 📈 Subtask 2.2: Increase Priority of an Existing Process
Alter your low-priority background worker (`12345`) directly in memory:
```bash
sudo renice -n -10 -p 12345
```
**Expected Output:**
```text
12345 (process ID) old priority 19, new priority -10
```

#### 🔍 Subtask 2.3: Verify Priority Change
Query the process structure to confirm modification changes:
```bash
ps -o pid,ni,cmd -p 12345
```

#### 🛡️ Subtask 2.4: Decrease Priority of User Processes
Throttle all background activities running under your current user scope:
```bash
renice -n 10 -u \$(whoami)
```

---

### 📊 Task 3: Monitoring Priority Impact on CPU Usage

#### 🖥️ Subtask 3.1: Launch Monitoring Tools
Track real-time engine processing speeds:
```bash
htop
```
* **Key Observation:** Processes configured with lower nice values (high priority) receive a much larger slice of CPU time.

#### 🏗️ Subtask 3.2: Create CPU Load
Spawn four concurrent background streams with increasing nice step gaps:
```bash
for i in {1..4}; do nice -n \$((i*5)) sha1sum /dev/zero & done
```

#### 📉 Subtask 3.3: Analyze in htop
1. Open `htop`.
2. Press `F6` to change sorting configurations and select priority.
3. Observe how the `%CPU` column correlates with the `NI` column metrics.

#### 🧹 Subtask 3.4: Cleanup Processes
Terminate the lab processing loads to prevent system slowdown:
```bash
pkill sha1sum
```

---

## 🏁 Conclusion
You have successfully completed the following milestones:
* Assigned specific runtime priorities at process initialization using `nice`.
* Rebalanced execution loads on existing processes using `renice`.
* Monitored scheduler behavioral changes using interactive dashboards.

### 💡 Key Takeaways
* **System Kernels:** Critical workloads typically consume negative nice spaces.
* **User Apps:** Desktop toolsets or computations should reside inside positive paths.
* **Performance:** Resource starvation can be actively controlled with scheduling tools.

---

## 🗺️ Additional Exercises
* **Telemetry Scripting:** Author a shell wrapper that tracks three custom processes and exports resource logs over time.
* **Advanced Controls:** Investigate Linux Control Groups (`cgroups`) for enterprise hardware limit pooling.
* **Services:** Adjust a system service unit file configuration to permanently inject custom values.

## 📚 References
* `man nice`
* `man renice`
* Linux Process Scheduling Core Documentation
