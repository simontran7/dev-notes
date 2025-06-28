
## Memory fragmentation

Memory fragmentation is a memory problem that occurs when the memory contains unusable blocks, which causes inefficient memory utilization and can eventually prevent successful allocation requests.

There are two types of memory fragmentation: internal memory fragmentation, and external memory fragmentation.
- Internal fragmentation occurs when the memory allocated to a process or object is larger than what was requested, leaving behind unused memory in the allocated block.
- External fragmentation happens when free memory becomes divided into many small, non-contiguous chunks over time, making it hard to allocate large contiguous blocks — even if the total free memory is enough.

https://excalidraw.com/#json=9Iw2dQgne9nH52gJsJpQg,Ytv_SNtGIDl8tBah1a8u6g