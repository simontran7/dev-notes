# Virtual Memory

## Memory Fragmentation

_Memory fragmentation_ is a memory problem that occurs when the memory contains unusable blocks, leading to inefficient memory utilization and can eventually prevent successful allocation requests.

There are two types of memory fragmentation:
- _Internal memory fragmentation_, which occurs when the memory allocated to a process or object is larger than what was requested, leaving behind unused memory in the allocated block.
- _External memory fragmentation_ happens when free memory becomes divided into many small, non-contiguous chunks over time, making it hard to allocate large contiguous blocks — even if the total free memory is enough.

https://excalidraw.com/#json=9Iw2dQgne9nH52gJsJpQg,Ytv_SNtGIDl8tBah1a8u6g