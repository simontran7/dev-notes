# Measuring Performance

## Latency Numbers

| Operation                          | Time (ns) | Time (ms) |
| ---------------------------------- | --------- | --------- |
| CPU cycle                          | 0.3 - 0.5 |           |
| L1 cache reference                 | 1         |           |
| Branch misprediction               | 3         |           |
| L2 cache reference                 | 4         |           |
| Mutex lock or unlock               | 17        |           |
| Main memory reference              | 100       |           |
| SSD Random Read                    | 17,000    | 0.017     |
| Read 1 MB sequentially from memory | 38,000    | 0.038     |
| Read 1 MB sequentially from SSD    | 622,000   | 0.622     |

[Source](https://chandlerc.blog/slides/2023-cppnow-compiler/index.html#/29)

## CPU Sampling Profiling

TO DO

## Benchmarking

### Simple

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

[Source](https://www.reddit.com/r/rust/comments/upehxv/comment/i8k9n8d/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)

### Advanced

```shell
hyperfine --warmup 3 '<program name>' # add this if you want to compare program '<program 2 name>'
```

### Hardware counters

```shell
# where # of runs = floor(<duration in ms> / <measured execution time of the first run>)
poop --duration <duration in ms> <program 1> <...> <program N>
```
