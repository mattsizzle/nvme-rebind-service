# nvme-rebind-service

systemd service to rebind a NVMe drive to the vfio-pci driver before Proxmox starts. \
I created this script as the `nvme` drive is built into the Proxmox kernel and I didn't \
want to modify the base system as puppet controls my host node lifecycles.

### Files

#### pve-manager.service ( The service unit defintion )

This file is placed at `/etc/systemd/system/pve-manager.service` and should be \
enabled at startup.

#### nvme0n1-rebind

This file is placed at `/usr/bin/nvme0n1-rebind` and handles this (un)binding \
and (re)mounting of a specific NVMe drive by toggling between the `nvme` and \
`vfio-pci` drivers. It needs to be executable.

### Notes

This script is not currently a copy pasta solution as it has hardcoded values. If you \
stumble upon this script and decided it meets your needs please ensure you change the \
values to match your system configuration.

### Example Output

    root@proxmox:~# systemctl status nvme0n1-rebind.service
        nvme0n1-rebind.service - Rebind the NVMe01 Drive to VFIO-PCI
        Loaded: loaded (/etc/systemd/system/nvme0n1-rebind.service; enabled; vendor preset: enabled)
        Active: inactive (dead) since Sun 2021-04-18 18:27:06 CDT; 3s ago
        Process: 17762 ExecStart=/usr/bin/nvme0n1-rebind bind (code=exited, status=0/SUCCESS)
        Process: 17779 ExecStop=/usr/bin/nvme0n1-rebind unbind (code=exited, status=0/SUCCESS)
        Main PID: 17762 (code=exited, status=0/SUCCESS)