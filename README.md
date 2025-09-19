# 1. System-Health-Monitoring-Script-Python-
This script checks CPU, memory, disk usage, and running processes, and logs alerts if thresholds are exceeded.

```

#!/usr/bin/env python3
import psutil
import logging
from datetime import datetime

# Configure logging
logging.basicConfig(filename='system_health.log',
                    level=logging.INFO,
                    format='%(asctime)s - %(levelname)s - %(message)s')

# Thresholds
CPU_THRESHOLD = 80   # percent
MEM_THRESHOLD = 80   # percent
DISK_THRESHOLD = 90  # percent

def check_cpu():
    usage = psutil.cpu_percent(interval=1)
    if usage > CPU_THRESHOLD:
        logging.warning(f"High CPU usage detected: {usage}%")
    return usage

def check_memory():
    mem = psutil.virtual_memory()
    if mem.percent > MEM_THRESHOLD:
        logging.warning(f"High Memory usage detected: {mem.percent}%")
    return mem.percent

def check_disk():
    disk = psutil.disk_usage('/')
    if disk.percent > DISK_THRESHOLD:
        logging.warning(f"High Disk usage detected: {disk.percent}%")
    return disk.percent

def check_processes():
    processes = [p.info for p in psutil.process_iter(['pid', 'name', 'cpu_percent'])]
    high_cpu_processes = [p for p in processes if p['cpu_percent'] > 50]
    if high_cpu_processes:
        logging.warning(f"Processes consuming high CPU: {high_cpu_processes}")
    return len(high_cpu_processes)

if __name__ == "__main__":
    cpu = check_cpu()
    mem = check_memory()
    disk = check_disk()
    high_cpu_proc_count = check_processes()

    print(f"CPU Usage: {cpu}%")
    print(f"Memory Usage: {mem}%")
    print(f"Disk Usage: {disk}%")
    print(f"Processes with high CPU usage: {high_cpu_proc_count}")
    print("System health check complete. Check 'system_health.log' for alerts.")
```

## Usage:

- python3 system_health.py

- Alerts will be written to system_health.log if any thresholds are exceeded.
