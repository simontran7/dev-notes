# RAM and CPU cache memory

## Memory bandwidth

### Non-temporal memory access optimization

Sometimes, we want to read or write cold data (i.e., data that is not expected to be reused soon). However, regular temporal loads or temporal stores can be inefficient in these cases. When performing a temporal load or store, the CPU typically loads the entire cache line containing the target address into the cache, modifies it if needed, and eventually writes it back to memory (in the case of a temporal store). This process can cause cache pollution: loading the cache line may evict warm or hot data (i.e., data likely to be used again soon) from the cache, reducing overall performance.

To avoid this inefficiency, modern processors support non-temporal load and store instructions. These operations allow cold data to be read from or written directly to memory while bypassing the cache hierarchy, preserving valuable cache space for data with higher temporal locality (i.e., data that is likely to be accessed repeatedly within a short period of time).