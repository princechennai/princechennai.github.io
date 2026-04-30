---
title: Linux Kernel Performance Tuning
description: Deep dive into optimizing Linux systems for high-throughput workloads
date: 2018-11-05
author: Prince Praveen
---

# 🚀 Linux Kernel Performance Tuning

Modern high-throughput systems don’t fail because of lack of hardware—they fail because of *inefficient defaults*. The Linux kernel is powerful, but its out-of-the-box configuration is designed for general-purpose workloads, not latency-sensitive or throughput-intensive production systems.

This guide walks through **practical kernel tuning techniques** used in real-world systems, along with a **data-driven benchmarking methodology** so you can validate improvements instead of relying on assumptions.

---

# 🧠 Tuning Philosophy

Before touching any kernel parameter:

- **Measure first, then tune**
- Change **one variable at a time**
- Validate using **repeatable benchmarks**
- Always have a **rollback strategy**

Kernel tuning without measurement is just guesswork.

---

# ⚙️ CPU & Scheduler Optimization

## 🧩 CPU Governor

Set CPU to performance mode to avoid frequency scaling latency:

```bash
echo performance | sudo tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
```

Or persist via:

```bash
sudo apt install cpufrequtils
echo 'GOVERNOR="performance"' | sudo tee /etc/default/cpufrequtils
```

---

## 🔁 Scheduler Selection

Check current scheduler:

```bash
cat /sys/block/sda/queue/scheduler
```

Common options:

- `mq-deadline` → balanced, good default for SSD
- `none` → best for NVMe devices (no scheduling overhead)
- `bfq` → desktop workloads

Set scheduler:

```bash
echo none | sudo tee /sys/block/nvme0n1/queue/scheduler
```

---

## 📌 CPU Pinning (Isolation)

Pin workloads to specific cores to reduce context switching:

```bash
taskset -c 2,3 ./your_app
```

Or isolate CPUs at boot:

```bash
GRUB_CMDLINE_LINUX="isolcpus=2,3 nohz_full=2,3 rcu_nocbs=2,3"
```

Then:

```bash
sudo update-grub
```

---

# 🧮 Memory Management Tuning

## 🔄 vm.swappiness

Controls how aggressively Linux swaps memory.

```bash
sysctl vm.swappiness=10
```

Persist:

```bash
echo "vm.swappiness=10" | sudo tee -a /etc/sysctl.conf
```

- `0–10` → latency-sensitive workloads
- `60` → default (too high for most production systems)

---

## 🧱 Transparent Huge Pages (THP)

THP can introduce latency spikes in some workloads (databases especially).

Disable:

```bash
echo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled
```

Persist via systemd:

```bash
sudo nano /etc/systemd/system/disable-thp.service
```

```ini
[Unit]
Description=Disable Transparent Huge Pages

[Service]
Type=oneshot
ExecStart=/bin/sh -c "echo never > /sys/kernel/mm/transparent_hugepage/enabled"

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl enable disable-thp
```

---

## 📊 vm.dirty_ratio & vm.dirty_background_ratio

Control when the kernel flushes dirty pages to disk.

```bash
sysctl vm.dirty_background_ratio=5
sysctl vm.dirty_ratio=10
```

- Lower values → more consistent latency
- Higher values → better throughput, worse spikes

---

# 💾 Disk & I/O Optimization

## ⚡ Read-Ahead Buffer

Check:

```bash
blockdev --getra /dev/sda
```

Set (example: 4096 KB):

```bash
blockdev --setra 4096 /dev/sda
```

- Larger values → good for sequential workloads
- Smaller values → better for random I/O

---

## 🧵 I/O Queue Depth

For NVMe:

```bash
cat /sys/block/nvme0n1/queue/nr_requests
```

Tune:

```bash
echo 1024 | sudo tee /sys/block/nvme0n1/queue/nr_requests
```

---

## 🔀 Multi-Queue (blk-mq)

Modern kernels use multi-queue by default, but verify:

```bash
cat /sys/block/sda/queue/nr_hw_queues
```

Higher values → better parallelism

---

# 🌐 Network Stack Optimization

## 📦 Increase Socket Buffers

```bash
sysctl -w net.core.rmem_max=134217728
sysctl -w net.core.wmem_max=134217728
```

---

## 🚀 TCP Tuning

```bash
sysctl -w net.ipv4.tcp_congestion_control=bbr
sysctl -w net.core.default_qdisc=fq
```

Enable BBR:

```bash
sysctl net.ipv4.tcp_available_congestion_control
```

---

## 🔌 File Descriptors

```bash
ulimit -n 1048576
```

Persist:

```bash
echo "* soft nofile 1048576" | sudo tee -a /etc/security/limits.conf
echo "* hard nofile 1048576" | sudo tee -a /etc/security/limits.conf
```

---

# 📏 Benchmarking Methodology

## 🧪 Tools

- `sysbench` → CPU, memory, I/O
- `fio` → disk benchmarking
- `iperf3` → network throughput
- `perf` → kernel-level profiling

---

## 📊 Example: Disk Benchmark

```bash
fio --name=randread \
    --ioengine=libaio \
    --rw=randread \
    --bs=4k \
    --numjobs=4 \
    --size=1G \
    --runtime=60 \
    --group_reporting
```

---

## 🧠 Key Metrics

Track:

- Throughput (MB/s)
- Latency (avg, p95, p99)
- CPU utilization
- Context switches
- I/O wait

---

## 🔁 Testing Strategy

1. Capture baseline
2. Apply **one tuning change**
3. Re-run benchmark
4. Compare results
5. Keep or rollback

---

# ⚠️ Common Pitfalls

- Tuning everything at once → impossible to isolate impact
- Ignoring workload type → wrong optimizations
- Blindly copying configs → dangerous in production
- Not testing under realistic load

---

# 🏁 Putting It All Together

A typical production-tuned system might include:

- CPU governor → `performance`
- Scheduler → `none` (NVMe)
- `vm.swappiness=10`
- THP disabled
- Tuned dirty ratios
- Increased I/O queue depth
- BBR congestion control
- High file descriptor limits

But the *exact combination depends on your workload*.

---

# 📌 Final Thoughts

Linux kernel tuning is not about memorizing sysctl values—it's about:

- Understanding system behavior
- Identifying bottlenecks
- Applying targeted optimizations
- Validating with data

The best engineers don’t just tune systems—they **prove improvements with evidence**.

---

# 🔗 Next Steps

If you're building high-performance platforms:

- Automate tuning via Ansible or Terraform
- Integrate benchmarks into CI pipelines
- Combine kernel tuning with observability (eBPF, perf, tracing)

---

**Measure. Tune. Validate. Repeat.**