# Testing

## Profiling

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