## Lab 4 — Partition, mirror, fail, recover

Pair: Inzhu + Diana · Date: 16.11.26 · Hypervisor: VirtualBox

## Windows 
'''PS C:\Users\Inzhu Diana> Get-Disk | Select Number, FriendlyName, PartitionStyle, Size

Number FriendlyName  PartitionStyle        Size
------ ------------  --------------        ----
     0 VBOX HARDDISK GPT            69020124160
     1 VBOX HARDDISK GPT             1073741824


PS C:\Users\Inzhu Diana> Get-Volume | Where FileSystemLabel -eq 'LAB04'

DriveLetter FriendlyName FileSystemType DriveType HealthStatus OperationalStatus SizeRemaining       Size
----------- ------------ -------------- --------- ------------ ----------------- -------------       ----
E           LAB04        NTFS           Fixed     Healthy      OK                    989.23 MB 1007.93 MB'''



## Linux — layout before and after
### Before

