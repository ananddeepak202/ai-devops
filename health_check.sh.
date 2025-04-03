#!/bin/bash

# Function to check CPU utilization
check_cpu() {
    cpu_idle=$(mpstat 1 1 | awk '/Average:/ {print $NF}')
    cpu_usage=$(echo "100 - $cpu_idle" | bc)
    if (( $(echo "$cpu_usage > 60" | bc -l) )); then
        echo "CPU utilization is $cpu_usage% (Not Healthy)"
        return 1
    else
        echo "CPU utilization is $cpu_usage% (Healthy)"
        return 0
    fi
}

# Function to check Memory utilization
check_memory() {
    memory_free=$(free | grep Mem | awk '{print $4/$2 * 100.0}')
    memory_usage=$(echo "100 - $memory_free" | bc)
    if (( $(echo "$memory_usage > 60" | bc -l) )); then
        echo "Memory utilization is $memory_usage% (Not Healthy)"
        return 1
    else
        echo "Memory utilization is $memory_usage% (Healthy)"
        return 0
    fi
}

# Function to check Disk utilization
check_disk() {
    disk_usage=$(df / | grep / | awk '{print $5}' | sed 's/%//g')
    if (( disk_usage > 60 )); then
        echo "Disk utilization is $disk_usage% (Not Healthy)"
        return 1
    else
        echo "Disk utilization is $disk_usage% (Healthy)"
        return 0
    fi
}

# Check the status of VM
check_cpu
cpu_status=$?
check_memory
memory_status=$?
check_disk
disk_status=$?

if (( cpu_status == 0 && memory_status == 0 && disk_status == 0 )); then
    echo "The VM is Healthy"
else
    echo "The VM is Not Healthy"
fi
