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

## 6. Other Forms of Fragmentation

### 6.1 Filesystem Fragmentation
- **Internal**: block-size waste  
- **External**: file stored in non-contiguous blocks  

Modern FS fix this with:
- Extents
- Preallocation
- Delayed allocation

### 6.2 Heap Fragmentation
Holes created by malloc/free patterns.

### 6.3 Paging Fragmentation
Internal fragmentation only.

### 6.4 Stack vs Heap
- Stack: no fragmentation  
- Heap: fragmentation builds over time  

### 6.5 GPU Fragmentation
Due to VRAM allocation.

## 7. How OS Reduces Fragmentation

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

## 8. Real Examples

1. File = 298B → block = 4096B → internal fragmentation  
2. Heap holes block larger allocs  
3. Last page partially used  
4. File stored non-contiguously  
5. Kernel uses slab caches  

## 9. Interview Questions (with answers)

### Q1: What is fragmentation?
Memory broken into unusable pieces.

### Q2: Types?
Internal, External.

### Q3: Difference?
Internal = inside block waste  
External = between blocks waste

### Q4: Why internal fragmentation?
Fixed block size.

### Q5: Why external?
Variable alloc/free.

### Q6: How OS avoids it?
Paging, compaction, slab.

### Q7: How ext4 avoids fragmentation?
Extents.

### Q8: Paging removes?
External: yes  
Internal: no

### Q9: Segmentation suffers?
Both.

### Q10: Examples of internal frag?
FS block waste, struct padding.

### Q11: External frag examples?
Heap holes, scattered file blocks.

### Q12: Can external frag be removed?
Paging removes it.

### Q13: Why slab allocator?
Fixed-size caches reduce holes.

### Q14: Compaction?
Move memory to form large free areas.

### Q15: Why no stack frag?
LIFO behavior.

### Q16: Why file occupies more than size?
Block allocation.

## 10. Summary

### Internal Fragmentation
Wasted inside block.

### External Fragmentation
Scattered free space.

### Solutions
Paging, extents, slab, compaction.
