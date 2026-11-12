8. Navigate to the **Disks** blade, and you will see both the data disks and the OS disk for the VM.

| Name | Storage account type | Size (G...) | Owner |
|---|---|---|---|
| data | Standard HDD LRS | 32 | sm029-az800-vm1 |
| logs | Premium SSD LRS | 4 | sm029-az800-vm1 |
| sm029-az800-vm1_OsDisk_1_7e2b81d46abc4ef5a598715b56d8b1c2 | Standard HDD LRS | 127 | sm029-az800-vm1 |

# Figure 11.18 – Data disks

In this task, we attached two data disks to a VM. In the next tasks, we will resize the created data disks.

# **Task 3 – resizing a data disk for a VM**

We will resize a disk for this exercise by deallocating the VM instead of detaching and reattaching:

+   1. Stop/deallocate your VM by clicking the **Stop** option.

![](bbox=[0.25, 0.58, 0.72, 0.72])

# Figure 11.19 – Stopping the VM