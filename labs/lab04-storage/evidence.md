## Lab 4 — Partition, mirror, fail, recover

Pair: Inzhu + Diana · Date: 16.11.26 · Hypervisor: VirtualBox

## Windows 
```powershell
PS C:\Users\Inzhu Diana> Get-Disk | Select Number, FriendlyName, PartitionStyle, Size

Number FriendlyName  PartitionStyle        Size
------ ------------  --------------        ----
     0 VBOX HARDDISK GPT            69020124160
     1 VBOX HARDDISK GPT             1073741824


PS C:\Users\Inzhu Diana> Get-Volume | Where FileSystemLabel -eq 'LAB04'

DriveLetter FriendlyName FileSystemType DriveType HealthStatus OperationalStatus SizeRemaining       Size
----------- ------------ -------------- --------- ------------ ----------------- -------------       ----
E           LAB04        NTFS           Fixed     Healthy      OK                    989.23 MB 1007.93 MB
```

### MBR or GPT: 
GPT, because windows uses UEFI that configurate with GPT. GPT stores backup partition table headers along with CRC32 checksums to protect against disk corruption.

## Linux — layout before and after
### Before

```powershell
NAME   SIZE TYPE FSTYPE MOUNTPOINTS
loop0  1.5M loop squashfs /snap/huctl/131
loop1 66.8M loop squashfs /snap/core24/1643
loop2  1.5M loop squashfs /snap/huctl/123
loop3 50.1M loop squashfs /snap/snapd/27710
sda     25G disk
├─sda1   1M part
└─sda2  25G part ext4   /
sdb      1G disk
sdc      1G disk
sdd      1G disk
sr0   1024M rom
```
### After 

```powershell
NAME    SIZE TYPE  FSTYPE            MOUNTPOINTS
loop0  66.8M loop  squashfs          /snap/core24/1643
loop1   1.5M loop  squashfs          /snap/huctl/123
loop2   1.5M loop  squashfs          /snap/huctl/131
loop3  50.1M loop  squashfs          /snap/snapd/27710
sda      25G disk
├─sda1    1M part
└─sda2   25G part  ext4              /
sdb       1G disk
└─sdb1 1022M part  linux_raid_member
  └─md0 1021M raid1 ext4             /mnt/mirror
sdc       1G disk
└─sdc1 1022M part  linux_raid_member
sdd       1G disk
└─sdd1 1022M part  linux_raid_member
  └─md0 1021M raid1 ext4             /mnt/mirror
sr0    1024M rom
```

## What i did 
### 1.First we made a hard disks for two VM's (1 GB each):
```powershell
for d in sdb sdc sdd; do
  sudo parted -s /dev/\$d mklabel gpt mkpart primary 1MiB 100% set 1 raid on
done
```
### 2.Create RAID 1 array, format, and mount
```powershell
sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb1 /dev/sdc1
sudo mkfs.ext4 -L mirror /dev/md0
sudo mkdir -p /mnt/mirror
sudo mount /dev/md0 /mnt/mirror
```
### 3.Simulate failure of sdc1 and remove it
```powershell
sudo mdadm /dev/md0 --fail /dev/sdc1
sudo mdadm /dev/md0 --remove /dev/sdc1
```
### 4.Add third disk (sdd1) to rebuild the mirror
```powershell
sudo mdadm /dev/md0 --add /dev/sdd1
sudo mdadm --detail --scan | sudo tee -a /etc/mdadm/mdadm.conf
sudo update-initramfs -u
```
## Three states 

| State | What | /proc/mdstat showed | File |

| --- | --- | --- |

| Healthy | [2/2] [UU] | evidence/array-healthy.txt |

| Degraded | [2/1] [U_] | evidence/array-degraded.txt |

| Rebuilt | [2/2] [UU], sdd1 replaced sdc1 | evidence/array-rebuilt.txt |
