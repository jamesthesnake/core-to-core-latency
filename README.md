Measuring CPU core-to-core latency
==================================

![License](https://img.shields.io/badge/license-MIT-green.svg)
[![Cargo](https://img.shields.io/crates/v/core-to-core-latency.svg)](https://crates.io/crates/core-to-core-latency)
[![Rust 1.57+](https://img.shields.io/badge/rust-1.57+-lightgray.svg)](https://www.rust-lang.org/tools/install)


We measure the latency it takes for a CPU to send a message to another CPU via
its cache coherence protocol.

By pinning two threads on two different CPU cores, we can get them to do a bunch
of compare-exchange operation, and measure the latency.

How to run:

```
$ cargo install core-to-core-latency
$ core-to-core-latency
```

Single socket results
----------------------

CPU                                                                            | Median Latency
-------------------------------------------------------------------------------| ------------------
Intel Core i9-12900K, 8+8 Cores, Alder Lake, 12th gen, 2021-Q4                 | 35ns, 44ns, 50ns
Intel Core i9-9900K, 3.60GHz, 8 Cores, Coffee Lake, 9th gen, 2018-Q4           | 21ns
Intel Core i7-1165G7, 2.80GHz, 4 Cores, Tiger Lake, 11th gen, 2020-Q3          | 27ns
Intel Core i7-6700K, 4.00GHz, 4 Cores, Skylake, 6th gen, 2015-Q3               | 27ns
Intel Core i5-10310U, 4 Cores, Comet Lake, 10th gen, 2020-Q2                   | 21ns
Intel Core i5-4590, 3.30GHz 4 Cores, Haswell, 4th gen, 2014-Q2                 | 21ns
Intel Xeon Platinum 8375C, 2.90GHz, 32 Cores, Ice Lake, 3rd gen, 2021-Q2       | 51ns
Intel Xeon Platinum 8275CL, 3.00GHz, 24 Cores, Cascade Lake, 2nd gen, 2019-Q2  | 47ns
Intel Xeon E5-2695 v4, 2.10GHz, 18 Cores, Broadwell, 5th gen, 2016-Q1          | 44ns
AMD EPYC 7R13, 48 Cores, Milan, 3rd gen, 2021-Q1                               | 23ns and 107ns
AMD Ryzen 9 5950X, 3.40GHz, 16 Cores, Zen3, 4th gen, 2020-Q4                   | 17ns and 85ns
AMD Ryzen 7 5700X, 3.40GHz, 8 Cores, Zen3, 4th gen, 2022-Q2                    | 18ns
AMD Ryzen 7 2700X, 3.70GHz, 8 Cores, Zen+, 2nd gen, 2018-Q3                    | 24ns and 92ns
AWS Graviton3, 64 Cores, Arm Neoverse, 3rd gen, 2021-Q4                        | 46ns
AWS Graviton2, 64 Cores, Arm Neoverse, 2rd gen, 2020-Q1                        | 47ns
IBM PowerPC 970, 1.8GHz, 2 Cores, 2003-Q2                                      | 576ns

## Intel Core i9-12900K, 8+8 Cores, Alder Lake, 12th gen, 2021-Q4

Data provided by [bizude](https://github.com/bizude).

This CPU has 8 performance cores, and 2 groups of 4 efficient cores.
We see CPU=8 with fast access to all other cores.

<img src="https://user-images.githubusercontent.com/297060/190930511-337ef53e-52c0-4350-9022-485689b7f242.png" width="530" />

## Intel Core i9-9900K, 3.60GHz, 8 Cores, Coffee Lake, 8th gen, 2018-Q4

My gaming machine, it's twice as fast as the other server-oriented CPUs.

<img src="https://user-images.githubusercontent.com/297060/190918912-8b551b33-14e6-4cd3-a82d-8ac241d1abb6.png" width="400" />

## Intel Core i7-1165G7, 2.80GHz, 4 Cores, Tiger Lake, 11th gen, 2020-Q3

Data provided by [Jonas Wunderlich](https://github.com/jonas-w)

<img src="https://user-images.githubusercontent.com/297060/190929491-aededfc2-e0f5-4e2f-9508-3eb154fd9bb1.png" width="450" />

## Intel Core i7-6700K, 4.00GHz, 4 Cores, Skylake, 6th gen, 2015-Q3

Data provided by [CanIGetaPR](https://github.com/CanIGetaPR)

<img src="https://user-images.githubusercontent.com/297060/190945571-a48078bc-0399-489a-81ea-271413aeec13.png" width="450" />

## Intel Core i5-10310U, 4 Cores, Comet Lake, 10th gen, 2020-Q2

Data provided by [Ashley Sommer](https://github.com/ashleysommer).

<img src = "https://user-images.githubusercontent.com/297060/190940870-526100e5-18bd-4a53-8d96-982627db581d.png" width="450" />

## Intel Core i5-4590, 3.30GHz, 4 Cores, Haswell, 4th gen, 2014-Q2

Data provided by [Felipe Lube de Bragança](https://github.com/felubra)

<img src="https://user-images.githubusercontent.com/297060/190928985-42e13598-f5dc-4b49-b67e-dc300207d3c7.png" width="450" />

## Intel Xeon Platinum 8375C, 2.90GHz 32 Cores, Ice Lake, 3rd gen, 2021-Q2

From an AWS `c6i.metal` machine.

<img src="https://user-images.githubusercontent.com/297060/190918865-7eaae192-6da6-41db-8faf-9496f6a7754b.png" width="1000" />

## Intel Xeon Platinum 8275CL, 3.00GHz 24 Cores, Cascade Lake, 2nd gen, 2019-Q2

From an AWS `c5.metal` machine.

<img src="https://user-images.githubusercontent.com/297060/190918895-8b90cc12-2e72-41d1-808b-6f03a8771898.png" width="700" />

## Intel Xeon E5-2695 v4, 2.10GHz 18 Cores, Broadwell, 5th gen, 2016-Q1

From a machine provided by GTHost

<img src="https://user-images.githubusercontent.com/297060/190918934-a2b11676-e6e1-4b88-a8e4-6bf69663d477.png" width="550" />

## AMD EPYC 7R13, 48 Cores, Milan, 3rd gen, 2021-Q1

From an AWS `c6a.metal` machine.

We can see cores arranged in 6 groups of 8 in which latency is excellent within
(23ns). When data crosses groups, the latency jumps to around 110ns. Note, that
the last 3 groups have a better cross-group latency than the first 3 (~90ns).

<img src="https://user-images.githubusercontent.com/297060/190893255-56ea9890-9e06-4f2d-bcef-249a70c4597b.png" width="1000" />

## AMD Ryzen 9 5950X, 3.40GHz 16 Cores, Zen3, 4th gen, 2020-Q1

Data provided by [John Schoenick](https://github.com/Nephyrin).

We can see 2 groups of 8 cores with latencies of 17ns intra-group, and 85ns inter-group.

<img src="https://user-images.githubusercontent.com/297060/190926938-400092a0-45ff-4a6c-816a-1b694767c993.png" width="530" />


## AMD Ryzen 7 5700X, 3.40GHz, 8 Cores, Zen3, 4th gen, 2022-Q2

Data provided by [Ashley Sommer](https://github.com/ashleysommer).

<img src="https://user-images.githubusercontent.com/297060/190940634-7c2b8beb-4630-4bfd-833b-df932808c8fb.png" width="400" />

## AMD Ryzen 7 2700X, 3.70GHz, 8 Cores, Zen+, 2nd gen, 2018-Q3

Data provided by [David Hoppenbrouwers](https://github.com/Demindiro).

We can see 2 groups of 4 cores with latencies of 24ns intra-group, and 92ns inter-group.

<img src="https://user-images.githubusercontent.com/297060/190931275-a4f8e842-a033-4438-9ceb-7f8a78951ec4.png" width="400" />

## AWS Graviton3, 64 Cores, Arm Neoverse, 3rd gen, 2021-Q4

From an AWS `c7g.16xlarge` machine.

<img src="https://user-images.githubusercontent.com/297060/190919040-7d6d2283-cbef-4544-8b07-f93f71754343.png" width="1000" />

## AWS Graviton2, 64 Cores, Arm Neoverse, 2nd gen, 2020-Q1

From an AWS `c6gd.metal` machine.

<img src="https://user-images.githubusercontent.com/297060/190919053-11480075-6731-49ce-af03-f50bb27e8b33.png" width="1000" />

Dual sockets results
---------------------

The following shows dual-socket configuration latency where one CPU on the first socket sends a message to
another CPU on the second socket.
The number in parenthesis next to the latency denotes the slowdown compared to single socket.

CPU                                                                            | Median Latency
-------------------------------------------------------------------------------| ------------------
Intel Xeon Platinum 8375C, 2.90GHz, 32 Cores, Ice Lake, 3rd gen, 2021-Q2       | 108ns (2.1x)
Intel Xeon Platinum 8275CL, 3.00GHz, 24 Cores, Cascade Lake, 2nd gen, 2019-Q2  | 134ns (2.8x)
Intel Xeon E5-2695 v4, 2.10GHz, 18 Cores, Broadwell, 5th gen, 2016-Q1          | 118ns (2.7x)
AMD EPYC 7R13, 48 Cores, Milan, 3rd gen, 2021-Q1                               | 197ns

## Dual Intel Xeon Platinum 8375C, 2.90GHz 32 Cores, Ice Lake, 3rd gen, 2021-Q2

From an AWS `c6i.metal` machine.

<img src="https://user-images.githubusercontent.com/297060/190943800-7c3a10b0-1ebb-4b49-b4a8-9f24cff5bd88.png" width="1000" />

## Dual Intel Xeon Platinum 8275CL, 3.00GHz 24 Cores, Cascade Lake, 2nd gen, 2019-Q2

From an AWS `c5.metal` machine.

<img src="https://user-images.githubusercontent.com/297060/190943799-80b844c1-3ddb-443f-82c4-20ddd2870130.png" width="1000" />

## Dual Intel Xeon E5-2695 v4, 2.10GHz 18 Cores, Broadwell, 5th gen, 2016-Q1

From a machine provided by GTHost

<img src="https://user-images.githubusercontent.com/297060/190943798-69b63b1f-42d5-496c-b527-6e301cb38086.png" width="1000" />

## Dual AMD EPYC 7R13, 48 Cores, Milan, 3rd gen, 2021-Q1

From an AWS `c6a.metal` machine.

This one is a bit odd. The single socket test for Socket 1 shows median latencies of 107ns
cross-groups, but Socket 2 shows 200ns. It's 2x slower, very odd. The other platforms don't behave this way.
In fact, the socket-to-socket latencies are than the core-to-core within Socket 2.

Anandtech have measured [similar results on a Dual-Socket AMD EPYC 7763 and 7742](https://www.anandtech.com/show/16529/amd-epyc-milan-review/4).

**Socket 2 does not behave similarly than Socket 1, it's twice as slow**.

<img src="https://user-images.githubusercontent.com/297060/190943333-3297c0aa-5d99-478a-8518-9eb6f96e4bc5.png" width="1000" />

Hyper-threads
-------------

We measure the latency between two hyper-threads of the same core

CPU                                                                            | Median Latency
-------------------------------------------------------------------------------| ------------------
Intel Core i9-12900K, 8+8 Cores, Alder Lake, 12th gen, 2021-Q4                 | 4.3ns
Intel Core i9-9900K, 3.60GHz, 8 Cores, Coffee Lake, 9th gen, 2018-Q4           | 6.2ns
Intel Core i7-1165G7, 2.80GHz, 4 Cores, Tiger Lake, 11th gen, 2020-Q3          | 5.9ns
Intel Core i7-6700K, 4.00GHz, 4 Cores, Skylake, 6th gen, 2015-Q3               | 6.9ns
Intel Core i5-10310U, 4 Cores, Comet Lake, 10th gen, 2020-Q2                   | 7.3ns
Intel Xeon Platinum 8375C, 2.90GHz, 32 Cores, Ice Lake, 3rd gen, 2021-Q2       | 8.1ns
Intel Xeon Platinum 8275CL, 3.00GHz, 24 Cores, Cascade Lake, 2nd gen, 2019-Q2  | 7.6ns
Intel Xeon E5-2695 v4, 2.10GHz, 18 Cores, Broadwell, 5th gen, 2016-Q1          | 7.6ns
AMD EPYC 7R13, 48 Cores, Milan, 3rd gen, 2021-Q1                               | 9.8ns
AMD Ryzen 9 5950X, 3.40GHz, 16 Cores, Zen3, 4th gen, 2020-Q4                   | 7.8ns
AMD Ryzen 7 5700X, 3.40GHz, 8 Cores, Zen3, 4th gen, 2022-Q2                    | 7.8ns
AMD Ryzen 7 2700X, 3.70GHz, 8 Cores, Zen+, 2nd gen, 2018-Q3                    | 9.7ns

---

**The notebook [results/results.ipynb](results/results.ipynb) contains the code to generate these graphs**

How to use
----------

First [install Rust](https://www.rust-lang.org/tools/install) and `gcc` on linux, then:

```
$ cargo install core-to-core-latency
$ core-to-core-latency
Num cores: 10
Using RDTSC to measure time: false
Num round trips per samples: 1000
Num samples: 300
Showing latency=round-trip-time/2 in nanoseconds:

       0       1       2       3       4       5       6       7       8       9
  0
  1   52±6
  2   38±6    39±4
  3   39±5    39±6    38±6
  4   34±6    38±4    37±6    36±5
  5   38±5    38±6    38±6    38±6    37±6
  6   38±5    37±6    39±6    36±4    49±6    38±6
  7   36±6    39±5    39±6    37±6    35±6    36±6    38±6
  8   37±5    38±6    35±5    39±5    38±6    38±5    37±6    37±6
  9   48±6    39±6    36±6    39±6    38±6    36±6    41±6    38±6    39±6

Min  latency: 34.5ns ±6.1 cores: (4,0)
Max  latency: 52.1ns ±9.4 cores: (1,0)
Mean latency: 38.4ns
```

Contribute
-----------

Use `core-to-core-latency 5000 --csv > output.csv` to instruct the program to use
5000 iterations per sample to reduce the noise, and save the results.

It can be used in the jupter notebook [results/results.ipynb](results/results.ipynb) for rendering graphs.

Create a GitHub issue with the generated `output.csv` file and I'll add your results.

License
-------

This software is licensed under the MIT license
