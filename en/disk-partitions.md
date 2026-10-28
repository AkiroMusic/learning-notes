# Disk Partition Types
### **I. System Boot and Bootstrapping Mechanism**

* **Boot derivation logic**: Press the power button $\rightarrow$ the system firmware (BIOS / UEFI) runs POST (power-on self-test, checking the status of hardware components) $\rightarrow$ it searches for the bootloader at a specific location on the storage device $\rightarrow$ loads and starts the operating system.
* **Key takeaway**: The bootloader ships with the operating system and is written to the disk during installation; the write location and partition scheme depend directly on the motherboard firmware type (BIOS corresponds to MBR, UEFI corresponds to GPT).

---

### **II. Partition Table Schemes: MBR and GPT**

#### **1. MBR (Master Boot Record)**
* **Core definition**: A partition scheme tied to legacy BIOS firmware; the bootloader and the partition table are stored in the first sector of the disk (only 512 bytes).
* **Derivation logic and limitations**:
  * **Addressing limit**: Constrained by BIOS and 32-bit addressing, it supports a maximum disk capacity of only **2 TB**; anything beyond that cannot be addressed or used.
  * **Quantity limit**: The partition table structure allows at most **4 primary partitions**.
  * **Single point of failure**: The critical boot and partition data exist in only a single copy; once the first sector is damaged, the system cannot boot at all.

#### **2. GPT (GUID Partition Table)**
* **Core definition**: A partition scheme tied to modern UEFI firmware; the space after the first 512 bytes (the protective MBR) is used to store the partition table and the address of the EFI System Partition.
* **Advantages and key takeaway**:
  * **Capacity and quantity breakthrough**: A single partition can be as large as **18 EB (exabytes)**, and on Windows a single disk supports up to **128 partitions** by default.
  * **Redundancy and fault tolerance**: Critical partition data is stored twice, at both the **beginning and the end** of the disk; if the primary partition table is damaged, it can be recovered automatically from the backup at the end of the disk, combining high speed with high reliability.

---

### **III. Core Functional Partitions in Detail**

#### **1. EFI System Partition (ESP)**
* **Core definition**: A hidden boot partition created automatically when installing an OS on a GPT disk; it has no drive letter and uses the **FAT32** file system (ensuring the UEFI firmware can read it directly).
* **Derivation logic**: UEFI reads the GPT partition table $\rightarrow$ locates the ESP $\rightarrow$ loads the OS boot files stored inside it $\rightarrow$ completes the system boot.
* **Key takeaway**: It supports the boot files of multiple operating systems coexisting within the same ESP, making it the core foundation for multi-OS / dual-boot setups on modern hardware.

#### **2. Swap Partition**
* **Core definition**: An area on a hard drive/SSD that serves as overflow for virtual memory, mainly used on Linux systems.
* **Derivation logic**: Physical memory (RAM) runs out $\rightarrow$ the system swaps out (Swap out) idle memory data to the swap partition $\rightarrow$ freeing physical memory to prevent a system crash $\rightarrow$ and swaps it back (Swap in) into physical memory when it is needed again.
* **Key takeaway**: Constrained by the read/write speed of the disk medium (far slower than RAM chips), heavy use of swap space makes the system noticeably sluggish.

#### **3. Recovery Partition**
* **Core definition**: A separate hidden partition that stores the Windows Recovery Environment (WinRE).
* **Derivation logic**: Physically isolated from the main system partition (insulated from main-system crashes or virus infections) $\rightarrow$ triggered automatically when the system fails to boot several times in a row $\rightarrow$ switches to this partition to start the recovery environment.
* **Key takeaway**: Provides emergency un-bricking capabilities such as low-level troubleshooting, uninstalling problematic updates, automatic boot repair, and resetting the operating system.

#### **4. Microsoft Reserved Partition (MSR)**
* **Core definition**: A hidden partition with no file system, generated automatically by modern Windows on GPT disks (the subtitle's 'Ms partition' actually means the MSR partition).
* **Derivation logic**: In normal use it simply reserves space $\rightarrow$ when the system performs special disk operations (such as converting a basic disk to a dynamic disk) $\rightarrow$ the MSR partition's capacity is automatically reduced $\rightarrow$ carving out space to store new disk-management metadata.
* **Key takeaway**: Invisible to users, but it provides the essential low-level space that Windows disk-management components and dynamic-volume operations rely on.

---

### **IV. The Legacy MBR Partition Scheme (Primary / Extended / Logical Partitions)**

#### **1. Primary Partition**
* **Core definition**: The basic partition on an MBR disk that can be accessed independently, store data, and host an operating system.
* **Derivation logic and takeaway**: A maximum of 4 per disk; the primary partition used to boot the system must be marked as the **active partition**, which contains the second-stage instructions of the MBR bootloader and is jumped to and executed directly by the BIOS once the first sector has been read.

#### **2. Extended Partition**
* **Core definition**: A special container structure designed to break through the MBR "4 primary partitions" limit.
* **Derivation logic and takeaway**: It consumes one of the primary-partition slots (creating the classic "**3 primary partitions + 1 extended partition**" cap); it cannot itself store data or host an operating system — its sole purpose is to act as a container within which logical partitions are created.

#### **3. Logical Partition**
* **Core definition**: A sub-partition created inside an extended partition.
* **Derivation logic and takeaway**: It depends on the extended partition for its existence and cannot be directly recognized by legacy BIOS as an independently bootable partition (it cannot be set as the active partition); it can be used for normal categorized data storage, or to install an operating system with the assistance of an existing primary bootloader — its core value lies in the flexibility of multi-partition data organization.
