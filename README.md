# 🤪  dull
> a dummy multi-thread HTTP server
> Chapter 20 of "The Rust Book".

## Performance

### Single threaded

The performance on a single-thread is quite interesting, considering it has to handle each connection sequentially.

```
λ echo "GET http://127.0.0.1:7878/" | vegeta attack -duration=10s -rate=1000 | vegeta report
Requests      [total, rate]            10000, 1000.05
Duration      [total, attack, wait]    9.999843606s, 9.999483s, 360.606µs
Latencies     [mean, 50, 95, 99, max]  371.656µs, 312.609µs, 595.447µs, 1.0544ms, 21.975149ms
Bytes In      [total, mean]            1760000, 176.00
Bytes Out     [total, mean]            0, 0.00
Success       [ratio]                  100.00%
Status Codes  [code:count]             200:10000
Error Set:
````
