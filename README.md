# Lab N — <Inventory your own machine>
Jazylbekova inzhu
Machine: <host>, <guest if relevant>
Date: 08.10.2026
## What I did
Steps, with commands in code blocks. Not prose about the steps.
## Result
| what | value | where |
| :--- | :--- | :--- |
|*CPU model*| AMD Ryzen 7 7445HS w/ Radeon 740M Graphics|(Get-CimInstance -ClassName Win32_Processor).Name|
|core count|6| (Get-CimInstance -ClassName Win32_Processor).NumberOfCores|
|thread count|12| (Get-CimInstance -ClassName Win32_Processor).ThreadCount|
|total memory|15,2617797851562|(Get-CimInstance Win32_ComputerSystem).TotalPhysicalMemory / 1GB|
|the number of installed modules|DeviceLocator Size (GB) Speed Manufacturer<br>------------- --------- ----- ------------<br>DIMM 0                8  5600 Samsung<br>DIMM 0                8  5600 Samsung |Get-CimInstance Win32_PhysicalMemory | Select-Object DeviceLocator, @{Name="Size (GB)";Expression={$_.Capacity / 1GB}}, Speed, Manufacturer|
|speed|DeviceLocator   Capacity Speed<br>-------------   -------- -----<br>DIMM 0        8589934592  5600<br>DIMM 0        8589934592  5600| Get-CimInstance -ClassName Win32_PhysicalMemory | Format-Table DeviceLocator, Capacity, Speed|
|disk model|WD PC SN5000S SDEQNSJ-1T00-1002|(Get-PhysicalDisk).FriendlyName|
|disk type|SSD / NVMe|(Get-PhysicalDisk).MediaType + " / " + (Get-Disk).BusType|
|free space on the volume|447,89|(Get-Volume -DriveLetter C).SizeRemaining / 1GB, 2)|
|firmware type|Uefi|(Get-ComputerInfo).BiosFirmwareType|
|version|FA707NUG.305|(Get-CimInstance Win32_BIOS).SMBIOSBIOSVersion|
|date|среда, 15 октября 2025 г. 05:00:00| (Get-CimInstance Win32_BIOS).ReleaseDate|
|hardware virtualization|True|(Get-CimInstance Win32_Processor).VirtualizationFirmwareEnabled|

## What did not work the first time
At least one thing. What I saw, what it turned out to be, what I did.
## Evidence
- [evidence/xxx.txt](evidence/xxx.txt) — one line on what this proves
