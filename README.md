# 🤪  dull
> a dummy multi-thread HTTP server
> Chapter 20 of "The Rust Book".

More info:
  * 🔩 [marceloboeira](https://github.com/marceloboeira)/[grab](https://github.com/marceloboeira/trpl) - The Rust Programming Language Annotations.

## Performance

### Single threaded

The performance on a single-thread is quite interesting, considering it has to handle each connection sequentially.

`λ echo "GET http://127.0.0.1:7878/" | vegeta attack -duration=1m -rate=500 | vegeta report`

```
Requests      [total, rate]            30000, 500.00
Duration      [total, attack, wait]    1m7.909608939s, 59.999667s, 7.909941939s
Latencies     [mean, 50, 95, 99, max]  1.831540351s, 738.542µs, 8.667226773s, 8.771782413s, 16.200500026s
Bytes In      [total, mean]            5212240, 173.74
Bytes Out     [total, mean]            0, 0.00
Success       [ratio]                  98.72%
Status Codes  [code:count]             0:385  200:29615
Error Set:
```

### Multi threaded

#### 3 threads

Less failures, same response time metrics, almost same await.

```
Requests      [total, rate]            30000, 498.50
Duration      [total, attack, wait]    1m8.154959971s, 1m0.180527s, 7.974432971s
Latencies     [mean, 50, 95, 99, max]  1.919961664s, 796.13µs, 8.635306402s, 8.767720613s, 16.796227919s
Bytes In      [total, mean]            5263984, 175.47
Bytes Out     [total, mean]            0, 0.00
Success       [ratio]                  99.70%
Status Codes  [code:count]             0:91  200:29909
```


#### 5 threads

Less failures, more await time and better max (longest request).
```
Requests      [total, rate]            30000, 500.00
Duration      [total, attack, wait]    1m13.239694331s, 59.999695s, 13.239999331s
Latencies     [mean, 50, 95, 99, max]  1.939717543s, 776.642µs, 8.651514717s, 8.714148167s, 14.717806331s
Bytes In      [total, mean]            5267856, 175.60
Bytes Out     [total, mean]            0, 0.00
Success       [ratio]                  99.77%
Status Codes  [code:count]             0:69  200:29931
```

#### 10 threads

Smaller await, same response times, ...
```
Requests      [total, rate]            30000, 500.00
Duration      [total, attack, wait]    1m7.85424267s, 59.999841s, 7.85440167s
Latencies     [mean, 50, 95, 99, max]  1.921707306s, 875.357µs, 8.664867293s, 8.767385647s, 9.007165635s
Bytes In      [total, mean]            5260288, 175.34
Bytes Out     [total, mean]            0, 0.00
Success       [ratio]                  99.63%
Status Codes  [code:count]             0:112  200:29888
```

#### 50 threads

More failures, less await, smaller response times....

```
Requests      [total, rate]            30000, 500.00
Duration      [total, attack, wait]    1m7.92261209s, 59.999957s, 7.92265509s
Latencies     [mean, 50, 95, 99, max]  1.762299338s, 687.111µs, 8.600099602s, 8.726004411s, 17.047197064s
Bytes In      [total, mean]            5126704, 170.89
Bytes Out     [total, mean]            0, 0.00
Success       [ratio]                  97.10%
Status Codes  [code:count]             0:871  200:29129
```

#### 10000 threads

Many more failures, same await, almost the same response times...
```
Requests      [total, rate]            30000, 500.00
Duration      [total, attack, wait]    1m7.858527123s, 59.999893s, 7.858634123s
Latencies     [mean, 50, 95, 99, max]  1.719965125s, 690.37µs, 8.657487979s, 8.817952196s, 16.966979238s
Bytes In      [total, mean]            5077248, 169.24
Bytes Out     [total, mean]            0, 0.00
Success       [ratio]                  96.16%
Status Codes  [code:count]             0:1152  200:28848
```
