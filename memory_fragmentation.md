#  MEMORY FRAGMENTATION — COMPLETE DETAILED NOTES

## 1. What is Memory Fragmentation?

Memory fragmentation happens when memory is divided into small unusable pieces that cannot satisfy allocation requests even though the total free memory is enough.

 **Total free memory is sufficient, but not *contiguous* or not *usable***.

Fragmentation occurs in:
- RAM
- Heap (malloc/new)
- Filesystems (ext4, NTFS)
- Disk storage
- GPU memory
- Embedded systems

## 2. Two Main Types of Fragmentation

```
1. Internal Fragmentation
2. External Fragmentation
```

## 3. INTERNAL FRAGMENTATION

### Definition
Wasted memory *inside* an allocated block.

Occurs when memory is allocated in **fixed-size blocks** and the requested memory does not completely use the allocated block.

### Example
Block size = 4 KB  
File size = 298 bytes

```
+--------------------------- 4096 bytes ----------------------------+
| 298 bytes used | 3798 bytes unused (WASTED)                      |
+------------------------------------------------------------------+
```

Unused space in the block = **internal fragmentation**.

### More examples
- Filesystems (ext4 4 KB block)
- Paging (last page partially filled)
- Network packets
- C structure padding
- Malloc alignment

### Key Points
- Happens *inside* the block  
- Waste is NOT usable  
- Caused by fixed block size  

## 4. EXTERNAL FRAGMENTATION

### Definition
Free memory exists, but it is split into scattered chunks that cannot form a large continuous allocation.

### Example
```
[10KB free] [used] [12KB free] [used] [15KB free]
```
Need 30 KB → allocation fails.

### Occurs in:
- Heap memory
- Long-running processes
- Kernel dynamic memory
- File systems without extents
- GPU memory

### Why it happens?
Alloc/free patterns create **holes**.

## 5. Comparison Table

| Feature           | Internal Fragmentation       | External Fragmentation              |
|------------------|------------------------------|-------------------------------------|
| Where occurs     | Inside allocated block       | In free memory space                |
| Cause            | Fixed block sizes            | Variable allocations/frees          |
| Waste type       | Inside block                 | Between blocks                      |
| Prevention       | Smaller blocks               | Paging, compaction, slab allocator  |



## 6. How OS Reduces Fragmentation

### Reduce internal fragmentation
- Smaller blocks  
- Buddy allocator  
- Variable block size  

### Reduce external fragmentation
- **Compaction**
- **Paging**
- Extents
- Free-list coalescing  

### Heap allocator techniques
- Binning
- Best-fit / first-fit
- Slab allocator

## 7. Real Examples

1. File = 298B → block = 4096B → internal fragmentation  
2. Heap holes block larger allocs  
3. Last page partially used  
4. File stored non-contiguously  
5. Kernel uses slab caches  

# Memory Fragmentation — Interview Q&A in One-Sentence Format

---

### **1. What is memory fragmentation?**  
Memory fragmentation is a condition where available memory becomes divided into small unusable pieces, preventing large contiguous allocations.

### **2. What are the two main types of memory fragmentation?**  
The two types are **internal fragmentation** and **external fragmentation**.

### **3. What is internal fragmentation?**  
Internal fragmentation occurs when a fixed-size allocated block is not fully used, causing wasted memory inside the block.

### **4. What causes internal fragmentation?**  
It is caused by **fixed block or page sizes** where the allocation is larger than the requested size.

### **5. What is external fragmentation?**  
External fragmentation occurs when free memory is available but scattered into non-contiguous chunks, preventing large allocations.

### **6. What causes external fragmentation?**  
It is caused by **variable-size allocations and deallocations** that leave holes in memory over time.

### **7. Give an example of internal fragmentation.**  
A 298-byte file stored in a 4096-byte block wastes the remaining 3798 bytes as internal fragmentation.

### **8. Give an example of external fragmentation.**  
A process requesting 30 KB fails even though free memory is 10 KB + 12 KB + 15 KB because it is not contiguous.

### **9. What is memory compaction?**  
Memory compaction is an OS technique that moves allocated blocks together to create one large contiguous free memory region, reducing external fragmentation.

### **10. Does paging eliminate fragmentation?**  
Paging eliminates **external fragmentation** but still causes **internal fragmentation** in the last page.

---

