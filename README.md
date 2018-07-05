HPB (Host-aware Performance Booster)
-----

  SW-centric solution that transfer HPB entry(hereinafter referred to as L2P entry) through COMMAND/RESPONSE UPIU.
  It brings UFS Performance improvement by using Host DRAM.
  
  
  
Overview
-----

  Host device driver caches L2P entries in host memory (DRAM) and sends the corresponding L2P information piggybacked in an I/O request to device whether the L2P entry cached in host memory.
  Since the L2P entry is provided by the host in the request, device does not have to load L2P entry from flash memory even on its internal L2P cache miss.
  Device investigates the host provided L2P to protect data against possible corruption of L2P metadata.
  In the following, we describe the HPB in more detail.
  
  - L2P Cache Initialization
  
    At boot time, HPB device driver allocates kernel memory for L2P cache, requests L2P information to device, and populates the L2P cache.

  - Device to Host L2P information delivery
  
    Host driver can fetch a chunk of L2P entries by sending a command, implemented via HPB_READ_BUFFER CMD.
    
  - Host to Device L2P information delivery
  
    HPB driver includes L2P information in an I/O request if host side cache has a corresponding L2P entry. 
    On receiving a read request with L2P information, device verifies if the given L2P has been published by itself, and checks whether that entry is up-to-date. 
    If the given L2P passes those inspections, device uses it without loading the entry from flash memory.



Terms
-----
  - HPB Region : Entire LBA range of a logical unit is equally divided by “bHPBRegionSize” in geometry descriptor
  - HPB Subregion : Each HPB Region includes multiple HPB SubRegions. Each HPB Region is equally divided by Size defined by bHPBSubRegionSize.
  - HPB Pinnedregion : Host can configure HPB Regions, like rarely updated address space (eg. OS), to be pinned for the L2P entries in those HPB Regions not to be changed. 
  - HPB Entry : Consists of PPN and its related device specific meta data. (= L2P entry)
  


For more datails
-----

  https://www.usenix.org/conference/hotstorage17/program/presentation/jeong
