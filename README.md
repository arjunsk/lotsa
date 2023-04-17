# lotsaa

`Lotsaa` = `Lotsa` + `a` fixed duration execution

This package was created by forking Josh Baker's [Lotsa](https://github.com/tidwall/lotsa) framework.

`Lotsaa` supports executing an operation on several threads for i) Fixed Operation Count ii) Fixed Duration.

## Install

```
go get -u github.com/arjunsk/lotsaa
```

## Example 1

To run 1,000,000 operations spread over 4 threads, use `lotsaa.Ops`

```go
var total int64
lotsaa.Output = os.Stdout
lotsaa.Ops(1000000, 4,
    func(i, thread int) {
        atomic.AddInt64(&total, 1)
    },
)
println(total)
```

Prints:

```
1,000,000 ops over 4 threads in 23ms, 43,580,037/sec, 22 ns/op
```

## Example 2

To run an operation spread over 4 threads for a fixed duration, use `lotsaa.Time`

```go
var total int64
lotsaa.Output = os.Stdout
lotsaa.Time(23 * time.Millisecond, 4,
    func(_ *rand.Rand, thread int) {
        atomic.AddInt64(&total, 1)
    },
)
```

Prints:

```
654,330 ops over 4 threads in 24ms, 27,207,775/sec, 36 ns/op
```

**NOTE 1**: The `ops metrics` differ between `lotsaa.Time` and `lotsaa.Ops` because, `lotsaa.Time` internally uses a counter to 
keep track of number of operations executed, and the latter doesn't. So, latter is showing higher ops. Hence, latter
can only be used for relative comparisons.

**NOTE 2**:
This will be useful for benchmarking **ephemeral** data structures like Cache 
(maybe a combination of [btree](https://github.com/tidwall/btree) + [hwt](https://github.com/RussellLuo/timingwheel)), 
where we can measure the overall read throughput (OPS) when all the interfering operations (`Reads`, `Writes`, and `Prunes`) act.

## License

Source code is available under the MIT [License](/LICENSE).
