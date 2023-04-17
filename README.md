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



## License

Source code is available under the MIT [License](/LICENSE).
