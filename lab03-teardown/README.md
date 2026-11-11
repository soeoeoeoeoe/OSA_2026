# Lab 3 – Teardown, and the same machine from the command line

## Part 1 

### AMD Ryzen 5 7600
https://github.com/soeoeoeoeoe/OSA_2026/blob/main/lab03-teardown/evidence/photos/AMD%20Ryzen%205%207600.jpg

**Caption:** 6-core / 12-thread desktop processor, base clock 3.8 GHz, boost up to 5.1 GHz, 65 W TDP, Zen 4 architecture.  
Connects to AM5 socket. Supports DDR5 memory.  
Limit: only works with AM5 motherboards.
Source: https://www.amd.com/en/search/site-search.html#q=amd%20ryzen%205%207600

### Intel Core i5-13400
https://github.com/soeoeoeoeoe/OSA_2026/blob/main/lab03-teardown/evidence/photos/INtel%20Core.webp

**Caption:**10-core (6P + 4E) / 16-thread desktop processor, base 2.5 GHz, boost up to 4.6 GHz, 65 W TDP.
Connects to LGA 1700 socket. Supports both DDR4 and DDR5 memor
Limit: Works only with LGA 1700 motherboards.Comes with Intel Laminar RM1 stock cooler.
Source:https://www.intel.com/content/www/us/en/support/ru-banner.html

### ASUS PRIME B650M-A
https://github.com/soeoeoeoeoe/OSA_2026/blob/main/lab03-teardown/evidence/photos/ASUS%20materinskaya%20plata.jpg

Caption: micro-ATX motherboard with AMD B650 chipset.Has AM5 socket and DDR5 memory slots (up to 4 DIMMs). Supports Ryzen 7000 series processors.
Limit: Only AM5 CPUs and DDR5 memory. Does not support Intel processors or DDR4 RAM.
Source:https://www.asus.com/

### MSI PRO B760M-A WIFI DDR4
https://github.com/soeoeoeoeoe/OSA_2026/blob/main/lab03-teardown/evidence/photos/MSI%20PRO.jpg
Caption: 16 GB DDR5 desktop memory module, speed 5600 MT/s, DIMM form factor.
Connects to DDR5 DIMM slots on the motherboard.
Limit: Works only on motherboards that support DDR5. Physically will not fit into DDR4 slots.
Source:https://www.msi.com/index.php

### Kingston FURY Beast DDR5-5600 16 GB
https://github.com/soeoeoeoeoe/OSA_2026/blob/main/lab03-teardown/evidence/photos/Kingston%20fury.jpg

Caption: 16 GB DDR5 desktop memory module, speed 5600 MT/s, DIMM form factor.
Connects to DDR5 DIMM slots on the motherboard.
Limit: Works only on motherboards that support DDR5. Physically will not fit into DDR4 slots.
Source:https://www.kingston.com/en/memory/gaming/kingston-fury-beast-ddr5-memory

### Crucial DDR4-3200 16 GB DIMM
https://github.com/soeoeoeoeoe/OSA_2026/blob/main/lab03-teardown/evidence/photos/Curcial.jpg
Caption: 16 GB DDR4 desktop memory module, speed 3200 MT/s, DIMM form factor.
Connects to DDR4 DIMM slots on the motherboard.
Limit: Works only on motherboards that support DDR4. Physically will not fit into DDR5 slots.

### Samsung 990 PRO 1 TB
https://github.com/soeoeoeoeoe/OSA_2026/blob/main/lab03-teardown/evidence/photos/Samsung%20990%20Pro.jpg
Caption: 1 TB NVMe SSD, PCIe 4.0 interface, M.2 2280 form factor.
Connects to an M.2 slot on the motherboard.
Limit: Requires an M.2 slot with PCIe support. Cannot be connected via SATA.
Source:https://www.samsung.com/us/

### Samsung 870 EVO 1 TB

Caption: 1 TB SATA SSD, 2.5-inch form factor.
Connects to a SATA port + SATA power cable.
Limit: Limited by SATA interface speed (maximum around 550 MB/s). Slower than NVMe drives.
Source:https://www.samsung.com/us/

### Corsair RM750e 750 W
https://github.com/soeoeoeoeoe/OSA_2026/blob/main/lab03-teardown/evidence/photos/Corsasiar.jpg
Caption: 750 W ATX power supply, 80 PLUS Gold efficiency, fully modular.
Connects to the motherboard via 24-pin ATX and CPU 8-pin (EPS) connectors. Also powers drives and graphics card.
Limit: Total system power consumption must stay under 750 W.
Source:https://www.corsair.com/us/en/p/psu/cp-9020295-na/rme-series-rm750e-fully-modular-low-noise-atx-power-supply-cp-9020295-na

### Stock cooler (Wraith Stealth)
Caption: AMD Wraith Stealth stock cooler that comes in the box with the Ryzen 5 7600.
Connects directly to the CPU (AM5 mounting).
Limit: Designed for 65 W TDP processors. Not suitable for high-power CPUs without good case airflow.
Source:https://www.amd.com/en/products/processors/ecosystem/cpu-cooler-solutions.html

---

## Two builds 

### Build 1
| Part | Model | Why it fits here |
|---|---|---|
| Processor | AMD Ryzen 5 7600 | AM5 socket |
| Motherboard | ASUS PRIME B650M-A | AM5 + DDR5 |
| Memory | Kingston FURY Beast DDR5-5600 | DDR5 supported |
| SSD | Samsung 990 PRO 1 TB | M.2 slot |
| Power Supply | Corsair RM750e | Enough wattage |

**What this build still needs:** case, GPU, second cooler (or keep the stock one), etc.

### Build 2
| Part | Model | Why it fits here |
|---|---|---|
| Processor | Intel Core i5-13400 | LGA 1700 |
| Motherboard | MSI PRO B760M-A WIFI DDR4 | LGA 1700 + DDR4  |
| Memory | Crucial DDR4-3200 | DDR4 supported |
| SSD | Samsung 870 EVO 1 TB | SATA or M.2 |
| Power Supply  | (or share) |       |


**What this build still needs:** a PC case, a discrete GPU (optional), a second RAM stick for dual-channel, and a CPU cooler (the stock cooler is assigned to the other build).

### Pictures:
In evidence/Photos
