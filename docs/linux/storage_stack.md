# Linux Sorage Stack

## 1. Physical layer
- Connection to storagedevice
- HBA (Host Bus Adapter): Physical plug-in card that accommodates the cable
- LUN (Logical Unit Number): Logical volume that presents the SAN on the server
- WWN (World Wide Number)

## 2. SCSI subsystem layer
- Linux kernel scans the Bus
- Block Devices: For every path to a LUN, a device is created
- !!! warning "When you're connected with 4 cables to the SAN, you see the same volume 4 times"

### SCSI (Small Computer System Interface)
- Universal language in which the computer and storage volumes communicate

## 3. Device mapper
- Framework
- Takes physical block devices and creates virtual, intelligent devices

### A. Multipathing
- Bundles raw paths to a single virtual device
- Advantage: If one cable fails, the DM seamlessly reroutes the traffic via the other cable.

### B. LVM (Logical Volume Manager)
- PV (Physical Volume)
- VG (Volume Group): Storage pool
- LV (Logical Volume): Partition
- Advantage: File systems can be enlarged, distributed across multiple disks, or snapshots can be created while the system is running.

### C. Encryption

## 4. Partition layer
- Storage space is divided into logical sections
- Partition tables:
  - MBR (Master Boot Record): Old, maximum 2TB, max. 4 primary partitions
  - GPT (GUID Partition Table): Modern, almost unlimited size, table redundancy
- `kpartx / partprobe`: Tools that inform the kernel that the partition table on a disk has changed.