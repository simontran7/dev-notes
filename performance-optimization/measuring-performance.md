# Measuring Performance

## Latency Numbers Reference

| Operation                          | Time (ns)     | Time (ms)  |
|------------------------------------|---------------|------------|
| CPU cycle                          | 0.3 - 0.5     |            |
| L1 cache reference                 | 1             |            |
| Branch misprediction               | 3             |            |
| L2 cache reference                 | 4             |            |
| Mutex lock or unlock               | 17            |            |
| Main memory reference              | 100           |            |
| SSD Random Read                    | 17,000        | 0.017      |
| Read 1 MB sequentially from memory | 38,000        | 0.038      |
| Read 1 MB sequentially from SSD    | 622,000       | 0.622      |

[Source](https://chandlerc.blog/slides/2023-cppnow-compiler/index.html#/29)

## Sampling Profiling

## Benchmarking

### Simple (good for most cases)

```rust
#[test] // so that you can run this with `cargo test --release -- my_bench --nocapture`
fn my_bench() {
    let t = std::time::Instant::now();
    for _ in 0..10 { // adjust the loop time manually, so that the total time is about 1 second
        let res = my_fn();
        // By checking result you make sure that compiler doesn't optimize it away,
        // and also ensure that your optimizations don't break semantics
        assert_eq!(res, expected_res)
    }
    // Print time with two digit after ., to avoid excessive precision
    eprintln!("{:.2?}", t.elapsed())
}
```