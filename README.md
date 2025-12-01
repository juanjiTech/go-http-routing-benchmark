Go HTTP Router Benchmark
========================

This benchmark suite aims to compare the performance of HTTP request routers for [Go](https://golang.org) by implementing the routing structure of some real world APIs.
Some of the APIs are slightly adapted, since they can not be implemented 1:1 in some of the routers.

Of course the tested routers can be used for any kind of HTTP request → handler function routing, not only (REST) APIs.


#### Tested routers & frameworks:

 * [Beego](http://beego.me/)
 * [go-json-rest](https://github.com/ant0ine/go-json-rest)
 * [Denco](https://github.com/naoina/denco)
 * [Gocraft Web](https://github.com/gocraft/web)
 * [Goji](https://github.com/zenazn/goji/)
 * [Gorilla Mux](http://www.gorillatoolkit.org/pkg/mux)
 * [http.ServeMux](http://golang.org/pkg/net/http/#ServeMux)
 * [HttpRouter](https://github.com/julienschmidt/httprouter)
 * [HttpTreeMux](https://github.com/dimfeld/httptreemux)
 * [Kocha-urlrouter](https://github.com/naoina/kocha-urlrouter)
 * [Martini](https://github.com/go-martini/martini)
 * [Pat](https://github.com/bmizerany/pat)
 * [Possum](https://github.com/mikespook/possum)
 * [R2router](https://github.com/vanng822/r2router)
 * [TigerTonic](https://github.com/rcrowley/go-tigertonic)
 * [Traffic](https://github.com/pilu/traffic)
 * [jin](https://github.com/juanjiTech/jin)


## Motivation

Go is a great language for web applications. Since the [default *request multiplexer*](http://golang.org/pkg/net/http/#ServeMux) of Go's net/http package is very simple and limited, an accordingly high number of HTTP request routers exist.

Unfortunately, most of the (early) routers use pretty bad routing algorithms. Moreover, many of them are very wasteful with memory allocations, which can become a problem in a language with Garbage Collection like Go, since every (heap) allocation results in more work for the Garbage Collector.

Lately more and more bloated frameworks pop up, outdoing one another in the number of features. This benchmark tries to measure their overhead.

Be aware that we are comparing apples and oranges here. We compare feature-rich frameworks to packages with simple routing functionality only. But since we are only interested in decent request routing, I think this is not entirely unfair. The frameworks are configured to do as little additional work as possible.

If you care about performance, this benchmark can maybe help you find the right router, which scales with your application.

Personally, I prefer slim and optimized software, which is why I implemented [HttpRouter](https://github.com/julienschmidt/httprouter), which is also tested here. In fact, this benchmark suite started as part of the packages tests, but was then extended to a generic benchmark suite.
So keep in mind, that I am not completely unbiased :relieved:


## Results

Benchmark System:
 * 13th Gen Intel(R) Core(TM) i7-13700KF
 * 128GB Memory
 * Windows 10 amd64
 * go version (1.24.0)


### Memory Consumption

Besides the micro-benchmarks, there are 3 sets of benchmarks where we play around with clones of some real-world APIs, and one benchmark with static routes only, to allow a comparison with [http.ServeMux](http://golang.org/pkg/net/http/#ServeMux).
The following table shows the memory required only for loading the routing structure for the respective API.
The best 3 values for each test are bold. I'm pretty sure you can detect a pattern :wink:

| Router           | Static      | GitHub       | Google+     | Parse       |
|:-----------------|------------:|-------------:|------------:|------------:|
| HttpServeMux     |   74240 B   |          -   |         -   |         -   |
| Ace              |   30648 B   |    48616 B   |     3680 B  |     6656 B  |
| Aero             |   33696 B   |   227696 B   |    26584 B  |    29328 B  |
| Bear             |   29984 B   |    80528 B   |     7096 B  |    12256 B  |
| Beego            |   98824 B   |   150952 B   |    10256 B  |    19256 B  |
| Bone             |   40480 B   |   100080 B   |     6688 B  |    11440 B  |
| Chi              |   83160 B   |    94888 B   |     8008 B  |     9656 B  |
| CloudyKitRouter  |   30376 B   |    93648 B   |     6728 B  |    11184 B  |
| Denco            |    9928 B   |    35360 B   |     3224 B  |     4080 B  |
| Echo             |   95752 B   |   121784 B   |    11416 B  |    14280 B  |
| Gin              |   34488 B   |    58280 B   |     4376 B  |     7696 B  |
| Gocraft Web      |   55088 B   |    93896 B   |     7480 B  |    12752 B  |
| Goji             |   29744 B   |    49680 B   |     3152 B  |     5680 B  |
| Gojiv2           |  118000 B   |   118640 B   |     8096 B  |    16064 B  |
| Go-Json-Rest     |  136184 B   |   141152 B   |    11448 B  |    14128 B  |
| GoRestful        |  803352 B   |  1254432 B   |    72168 B  |   119936 B  |
| Gorilla Mux      |  599496 B   |  1319696 B   |    68000 B  |   105384 B  |
| GowwwRouter      |   24512 B   |    86656 B   |     6168 B  |    10048 B  |
| HttpRouter       |   21680 B   |    37072 B   |     2776 B  |     5024 B  |
| HttpTreeMux      |   73448 B   |    78800 B   |     7440 B  |     7848 B  |
| Jin              |   35272 B   |    60112 B   |     4344 B  |     7768 B  |
| Kocha            |  123160 B   |   784144 B   |   128880 B  |   181712 B  |
| LARS             |   30640 B   |    48592 B   |     3656 B  |     6632 B  |
| Macaron          |   36976 B   |    90632 B   |     8672 B  |    13704 B  |
| Martini          |  314360 B   |   474792 B   |    24656 B  |    43760 B  |
| Pat              |   20200 B   |    19400 B   |     1848 B  |     2552 B  |
| Possum           |   88672 B   |    81832 B   |     7192 B  |     8104 B  |
| R2router         |   23256 B   |    46616 B   |     3864 B  |     6920 B  |
| Rivet            |   24608 B   |    42840 B   |     3064 B  |     5680 B  |
| Tango            |   28296 B   |    54872 B   |     5200 B  |     8952 B  |
| TigerTonic       |   78328 B   |    94400 B   |     9392 B  |     9808 B  |
| Traffic          |  555248 B   |   916720 B   |    48176 B  |    78832 B  |
| Vulcan           |  367064 B   |   423360 B   |    25448 B  |    44024 B  |

The first place goes to [Pat](https://github.com/bmizerany/pat), followed by [HttpRouter](https://github.com/julienschmidt/httprouter) and [Goji](https://github.com/zenazn/goji/). Now, before everyone starts reading the documentation of Pat, `[SPOILER]` this low memory consumption comes at the price of relatively bad routing performance. The routing structure of Pat is simple - probably too simple. `[/SPOILER]`.

Moreover main memory is cheap and usually not a scarce resource. As long as the router doesn't require Megabytes of memory, it should be no deal breaker. But it gives us a first hint how efficient or wasteful a router works.


### Static Routes

The `Static` benchmark is not really a clone of a real-world API. It is just a collection of random static paths inspired by the structure of the Go directory. It might not be a realistic URL-structure.

The only intention of this benchmark is to allow a comparison with the default router of Go's net/http package, [http.ServeMux](http://golang.org/pkg/net/http/#ServeMux), which is limited to static routes and does not support parameters in the route pattern.

In the `StaticAll` benchmark each of 157 URLs is called once per repetition (op, *operation*). If you are unfamiliar with the `go test -bench` tool, the first number is the number of repetitions the `go test` tool made, to get a test running long enough for measurements. The second column shows the time in nanoseconds that a single repetition takes. The third number is the amount of heap memory allocated in bytes, the last one the average number of allocations made per repetition.

The logs below show, that http.ServeMux has only medium performance, compared to more feature-rich routers. The fastest router only needs 1.8% of the time http.ServeMux needs.

[HttpRouter](https://github.com/julienschmidt/httprouter) was the first router (I know of) that managed to serve all the static URLs without a single heap allocation. Since [the first run of this benchmark](https://github.com/julienschmidt/go-http-routing-benchmark/blob/0eb78904be13aee7a1e9f8943386f7c26b9d9d79/README.md) more routers followed this trend and were optimized in the same way.

```
BenchmarkHttpServeMux_StaticAll       61789     19082 ns/op           0 B/op        0 allocs/op

BenchmarkAce_StaticAll                163642      7329 ns/op           0 B/op        0 allocs/op
BenchmarkAero_StaticAll               407326      2984 ns/op           0 B/op        0 allocs/op
BenchmarkBear_StaticAll                45849     26784 ns/op       19960 B/op      469 allocs/op
BenchmarkBeego_StaticAll               12692     93474 ns/op       65920 B/op      944 allocs/op
BenchmarkBone_StaticAll                51690     23267 ns/op           0 B/op        0 allocs/op
BenchmarkChi_StaticAll                 21536     52881 ns/op       57776 B/op      314 allocs/op
BenchmarkCloudyKitRouter_StaticAll    201948      5916 ns/op           0 B/op        0 allocs/op
BenchmarkDenco_StaticAll              509046      2338 ns/op           0 B/op        0 allocs/op
BenchmarkEcho_StaticAll               158776      7732 ns/op           0 B/op        0 allocs/op
BenchmarkGin_StaticAll                145191      8236 ns/op           0 B/op        0 allocs/op
BenchmarkGocraftWeb_StaticAll          28305     42175 ns/op       45056 B/op      785 allocs/op
BenchmarkGoji_StaticAll                57685     20779 ns/op           0 B/op        0 allocs/op
BenchmarkGojiv2_StaticAll               10000    117564 ns/op      175840 B/op     1099 allocs/op
BenchmarkGoJsonRest_StaticAll           16836     71219 ns/op       46629 B/op     1727 allocs/op
BenchmarkGoRestful_StaticAll             2104    582514 ns/op      632609 B/op     2193 allocs/op
BenchmarkGorillaMux_StaticAll            3205    357652 ns/op      133137 B/op     1099 allocs/op
BenchmarkGowwwRouter_StaticAll         175012      7005 ns/op           0 B/op        0 allocs/op
BenchmarkHttpRouter_StaticAll         264099      4580 ns/op           0 B/op        0 allocs/op
BenchmarkHttpTreeMux_StaticAll        232586      4572 ns/op           0 B/op        0 allocs/op
BenchmarkJin_StaticAll                 24658     47892 ns/op       10048 B/op      314 allocs/op
BenchmarkKocha_StaticAll              215935      5535 ns/op           0 B/op        0 allocs/op
BenchmarkLARS_StaticAll                180042      6637 ns/op           0 B/op        0 allocs/op
BenchmarkMacaron_StaticAll              10000    104583 ns/op      114296 B/op     1256 allocs/op
BenchmarkMartini_StaticAll               1820    658034 ns/op      129209 B/op     2031 allocs/op
BenchmarkPat_StaticAll                   1296    895554 ns/op      602833 B/op    12559 allocs/op
BenchmarkPossum_StaticAll               25065     48304 ns/op       65312 B/op      471 allocs/op
BenchmarkR2router_StaticAll            53516     22705 ns/op       17584 B/op      471 allocs/op
BenchmarkRivet_StaticAll               142369      8598 ns/op           0 B/op        0 allocs/op
BenchmarkTango_StaticAll               15810     75583 ns/op       31272 B/op      942 allocs/op
BenchmarkTigerTonic_StaticAll          83220     14457 ns/op        7376 B/op      157 allocs/op
BenchmarkTraffic_StaticAll               1485    790888 ns/op      749839 B/op    14444 allocs/op
BenchmarkVulcan_StaticAll               23126     51945 ns/op       15386 B/op      471 allocs/op
```

### Micro Benchmarks

The following benchmarks measure the cost of some very basic operations.

In the first benchmark, only a single route, containing a parameter, is loaded into the routers. Then a request for a URL matching this pattern is made and the router has to call the respective registered handler function. End.
```
BenchmarkAce_Param                 20646332        61.63 ns/op        32 B/op        1 allocs/op
BenchmarkAero_Param                62312413        19.31 ns/op         0 B/op        0 allocs/op
BenchmarkBear_Param                  3637874       324.3 ns/op       456 B/op        5 allocs/op
BenchmarkBeego_Param                 2330830       502.5 ns/op       400 B/op        6 allocs/op
BenchmarkBone_Param                  2527646       475.0 ns/op       752 B/op        5 allocs/op
BenchmarkChi_Param                   2794821       435.8 ns/op       704 B/op        4 allocs/op
BenchmarkCloudyKitRouter_Param      83811172        13.77 ns/op         0 B/op        0 allocs/op
BenchmarkDenco_Param                25366653        48.15 ns/op        32 B/op        1 allocs/op
BenchmarkEcho_Param                  47830869        24.79 ns/op         0 B/op        0 allocs/op
BenchmarkGin_Param                   37535424        32.31 ns/op         0 B/op        0 allocs/op
BenchmarkGocraftWeb_Param            2751200       422.0 ns/op       640 B/op        8 allocs/op
BenchmarkGoji_Param                  5219900       227.0 ns/op       336 B/op        2 allocs/op
BenchmarkGojiv2_Param                1827607       640.1 ns/op      1136 B/op        8 allocs/op
BenchmarkGoJsonRest_Param            2422519       496.1 ns/op       617 B/op       13 allocs/op
BenchmarkGoRestful_Param              580418      2234 ns/op      4312 B/op       15 allocs/op
BenchmarkGorillaMux_Param            1479970       822.1 ns/op      1152 B/op        8 allocs/op
BenchmarkGowwwRouter_Param           6302335       186.5 ns/op       368 B/op        2 allocs/op
BenchmarkHttpRouter_Param           30417093        37.11 ns/op        32 B/op        1 allocs/op
BenchmarkHttpTreeMux_Param           5713561       209.3 ns/op       352 B/op        3 allocs/op
BenchmarkJin_Param                   4653404       276.9 ns/op        64 B/op        2 allocs/op
BenchmarkKocha_Param                13925556        87.72 ns/op        56 B/op        3 allocs/op
BenchmarkLARS_Param                  47317678        24.14 ns/op         0 B/op        0 allocs/op
BenchmarkMacaron_Param               1303544       897.6 ns/op      1064 B/op       10 allocs/op
BenchmarkMartini_Param                706018      1843 ns/op      1096 B/op       12 allocs/op
BenchmarkPat_Param                   2178472       548.0 ns/op       552 B/op       12 allocs/op
BenchmarkPossum_Param                2947329       401.3 ns/op       496 B/op        5 allocs/op
BenchmarkR2router_Param              5299088       224.8 ns/op       400 B/op        4 allocs/op
BenchmarkRivet_Param                 20805231        55.64 ns/op        48 B/op        1 allocs/op
BenchmarkTango_Param                 3868410       309.2 ns/op       192 B/op        6 allocs/op
BenchmarkTigerTonic_Param            1432802       837.6 ns/op       744 B/op       16 allocs/op
BenchmarkTraffic_Param                910442      1414 ns/op      1888 B/op       23 allocs/op
BenchmarkVulcan_Param                7352332       166.9 ns/op        98 B/op        3 allocs/op
```

Same as before, but now with multiple parameters, all in the same single route. The intention is to see how the routers scale with the number of parameters. The values of the parameters must be passed to the handler function somehow, which requires allocations. Let's see how clever the routers solve this task with a route containing 5 and 20 parameters:
```
BenchmarkAce_Param5                  8572707       137.4 ns/op       160 B/op        1 allocs/op
BenchmarkAero_Param5                 31502346        36.12 ns/op         0 B/op        0 allocs/op
BenchmarkBear_Param5                  2555323       467.2 ns/op       501 B/op        5 allocs/op
BenchmarkBeego_Param5                 1929915       622.6 ns/op       448 B/op        6 allocs/op
BenchmarkBone_Param5                  1888977       625.9 ns/op       800 B/op        5 allocs/op
BenchmarkChi_Param5                   1980265       605.7 ns/op       704 B/op        4 allocs/op
BenchmarkCloudyKitRouter_Param5       25643216        46.71 ns/op         0 B/op        0 allocs/op
BenchmarkDenco_Param5                  9173350       127.5 ns/op       160 B/op        1 allocs/op
BenchmarkEcho_Param5                  19322582        61.51 ns/op         0 B/op        0 allocs/op
BenchmarkGin_Param5                   17295027        67.48 ns/op         0 B/op        0 allocs/op
BenchmarkGocraftWeb_Param5             1812714       683.1 ns/op       912 B/op       11 allocs/op
BenchmarkGoji_Param5                  3736128       313.8 ns/op       336 B/op        2 allocs/op
BenchmarkGojiv2_Param5                1561311       749.6 ns/op      1200 B/op        8 allocs/op
BenchmarkGoJsonRest_Param5            1240323       964.5 ns/op      1065 B/op       16 allocs/op
BenchmarkGoRestful_Param5              583118      2475 ns/op      4424 B/op       15 allocs/op
BenchmarkGorillaMux_Param5            1000000      1225 ns/op      1216 B/op        8 allocs/op
BenchmarkGowwwRouter_Param5           5600352       222.1 ns/op       368 B/op        2 allocs/op
BenchmarkHttpRouter_Param5           10931589       109.6 ns/op       160 B/op        1 allocs/op
BenchmarkHttpTreeMux_Param5           2674272       445.8 ns/op       576 B/op        6 allocs/op
BenchmarkJin_Param5                   4269564       296.7 ns/op        64 B/op        2 allocs/op
BenchmarkKocha_Param5                 3136383       381.0 ns/op       440 B/op       10 allocs/op
BenchmarkLARS_Param5                  24283976        49.07 ns/op         0 B/op        0 allocs/op
BenchmarkMacaron_Param5               1000000      1030 ns/op      1064 B/op       10 allocs/op
BenchmarkMartini_Param5                555103      2124 ns/op      1256 B/op       13 allocs/op
BenchmarkPat_Param5                   1000000      1365 ns/op      1008 B/op       29 allocs/op
BenchmarkPossum_Param5                2920383       400.8 ns/op       496 B/op        5 allocs/op
BenchmarkR2router_Param5              4487577       264.2 ns/op       400 B/op        4 allocs/op
BenchmarkRivet_Param5                 6340288       186.8 ns/op       240 B/op        1 allocs/op
BenchmarkTango_Param5                 3020052       402.9 ns/op       312 B/op        6 allocs/op
BenchmarkTigerTonic_Param5             480607      2905 ns/op      2408 B/op       36 allocs/op
BenchmarkTraffic_Param5                611642      2272 ns/op      2400 B/op       32 allocs/op
BenchmarkVulcan_Param5                5153248       239.4 ns/op        98 B/op        3 allocs/op

BenchmarkAce_Param20                  2906481       407.1 ns/op       704 B/op        1 allocs/op
BenchmarkAero_Param20                 84854227        14.30 ns/op         0 B/op        0 allocs/op
BenchmarkBear_Param20                  824458      1510 ns/op      1658 B/op        7 allocs/op
BenchmarkBeego_Param20                1000000      1353 ns/op       544 B/op        7 allocs/op
BenchmarkBone_Param20                  832194      1573 ns/op      1960 B/op        7 allocs/op
BenchmarkChi_Param20                   567762      2477 ns/op      2504 B/op        9 allocs/op
BenchmarkCloudyKitRouter_Param20       3610263       365.3 ns/op         0 B/op        0 allocs/op
BenchmarkDenco_Param20                 2508020       475.2 ns/op       704 B/op        1 allocs/op
BenchmarkEcho_Param20                  7148196       168.8 ns/op         0 B/op        0 allocs/op
BenchmarkGin_Param20                   6700600       162.6 ns/op         0 B/op        0 allocs/op
BenchmarkGocraftWeb_Param20            493606      2723 ns/op      3736 B/op       18 allocs/op
BenchmarkGoji_Param20                 1260721       943.0 ns/op      1240 B/op        4 allocs/op
BenchmarkGojiv2_Param20               1246036       957.0 ns/op      1440 B/op        8 allocs/op
BenchmarkGoJsonRest_Param20            358734      3613 ns/op      4529 B/op       23 allocs/op
BenchmarkGoRestful_Param20             286122      4513 ns/op      6720 B/op       20 allocs/op
BenchmarkGorillaMux_Param20            410716      2807 ns/op      3272 B/op       13 allocs/op
BenchmarkGowwwRouter_Param20          2520817       474.3 ns/op       368 B/op        2 allocs/op
BenchmarkHttpRouter_Param20            3237376       370.5 ns/op       704 B/op        1 allocs/op
BenchmarkHttpTreeMux_Param20           491570      2474 ns/op      3144 B/op       13 allocs/op
BenchmarkJin_Param20                  3192848       383.8 ns/op        64 B/op        2 allocs/op
BenchmarkKocha_Param20                 998011      1364 ns/op      1872 B/op       27 allocs/op
BenchmarkLARS_Param20                 12021843        98.68 ns/op         0 B/op        0 allocs/op
BenchmarkMacaron_Param20               434710      2807 ns/op      2864 B/op       15 allocs/op
BenchmarkMartini_Param20               306057      3959 ns/op      3568 B/op       18 allocs/op
BenchmarkPat_Param20                   219102      5706 ns/op      4752 B/op       83 allocs/op
BenchmarkPossum_Param20               2954390       403.8 ns/op       496 B/op        5 allocs/op
BenchmarkR2router_Param20              874851      1582 ns/op      2200 B/op        9 allocs/op
BenchmarkRivet_Param20                1430050       837.7 ns/op      1024 B/op        1 allocs/op
BenchmarkTango_Param20                1336596       897.3 ns/op       872 B/op        6 allocs/op
BenchmarkTigerTonic_Param20            111576     11472 ns/op     10032 B/op       89 allocs/op
BenchmarkTraffic_Param20               156039      7810 ns/op      8416 B/op       60 allocs/op
BenchmarkVulcan_Param20               2496652       496.5 ns/op        98 B/op        3 allocs/op
```

Now let's see how expensive it is to access a parameter. The handler function reads the value (by the name of the parameter, e.g. with a map lookup; depends on the router) and writes it to our [web scale storage](https://www.youtube.com/watch?v=b2F-DItXtZs) (`/dev/null`).
```
BenchmarkAce_ParamWrite              14112350        90.77 ns/op        40 B/op        2 allocs/op
BenchmarkAero_ParamWrite             44037505        26.65 ns/op         0 B/op        0 allocs/op
BenchmarkBear_ParamWrite              3676364       320.0 ns/op       456 B/op        5 allocs/op
BenchmarkBeego_ParamWrite             2296898       520.9 ns/op       408 B/op        7 allocs/op
BenchmarkBone_ParamWrite              2411034       501.5 ns/op       752 B/op        5 allocs/op
BenchmarkChi_ParamWrite               2660726       447.4 ns/op       704 B/op        4 allocs/op
BenchmarkCloudyKitRouter_ParamWrite  86483992        13.31 ns/op         0 B/op        0 allocs/op
BenchmarkDenco_ParamWrite            23377912        53.06 ns/op        32 B/op        1 allocs/op
BenchmarkEcho_ParamWrite             28076874        49.86 ns/op         8 B/op        1 allocs/op
BenchmarkGin_ParamWrite              27391068        42.68 ns/op         0 B/op        0 allocs/op
BenchmarkGocraftWeb_ParamWrite        2569024       454.9 ns/op       648 B/op        9 allocs/op
BenchmarkGoji_ParamWrite              5157429       237.6 ns/op       336 B/op        2 allocs/op
BenchmarkGojiv2_ParamWrite            1662300       721.5 ns/op      1168 B/op       10 allocs/op
BenchmarkGoJsonRest_ParamWrite        1493882       801.0 ns/op      1096 B/op       18 allocs/op
BenchmarkGoRestful_ParamWrite         574129      2254 ns/op      4320 B/op       16 allocs/op
BenchmarkGorillaMux_ParamWrite        1424162       833.3 ns/op      1152 B/op        8 allocs/op
BenchmarkGowwwRouter_ParamWrite      2317621       513.5 ns/op       784 B/op        6 allocs/op
BenchmarkHttpRouter_ParamWrite       25246627        44.75 ns/op        32 B/op        1 allocs/op
BenchmarkHttpTreeMux_ParamWrite       5279061       216.7 ns/op       352 B/op        3 allocs/op
BenchmarkJin_ParamWrite               4240654       297.9 ns/op        64 B/op        2 allocs/op
BenchmarkKocha_ParamWrite            12576730        90.93 ns/op        56 B/op        3 allocs/op
BenchmarkLARS_ParamWrite             36107600        33.52 ns/op         0 B/op        0 allocs/op
BenchmarkMacaron_ParamWrite          1000000      1060 ns/op      1136 B/op       14 allocs/op
BenchmarkMartini_ParamWrite           637202      2043 ns/op      1168 B/op       16 allocs/op
BenchmarkPat_ParamWrite               1413882       847.6 ns/op       976 B/op       16 allocs/op
BenchmarkPossum_ParamWrite            2918832       402.3 ns/op       496 B/op        5 allocs/op
BenchmarkR2router_ParamWrite          5172952       232.7 ns/op       400 B/op        4 allocs/op
BenchmarkRivet_ParamWrite            12838041        96.22 ns/op       112 B/op        2 allocs/op
BenchmarkTango_ParamWrite             7620788       157.3 ns/op        96 B/op        3 allocs/op
BenchmarkTigerTonic_ParamWrite       1000000      1203 ns/op      1184 B/op       21 allocs/op
BenchmarkTraffic_ParamWrite           818206      1788 ns/op      2312 B/op       27 allocs/op
BenchmarkVulcan_ParamWrite            7139712       171.2 ns/op        98 B/op        3 allocs/op
```

### [Parse.com](https://parse.com/docs/rest#summary)

Enough of the micro benchmark stuff. Let's play a bit with real APIs. In the first set of benchmarks, we use a clone of the structure of [Parse](https://parse.com)'s decent medium-sized REST API, consisting of 26 routes.

The tasks are 1.) routing a static URL (no parameters), 2.) routing a URL containing 1 parameter, 3.) same with 2 parameters, 4.) route all of the routes once (like the StaticAll benchmark, but the routes now contain parameters).

Worth noting is, that the requested route might be a good case for some routing algorithms, while it is a bad case for another algorithm. The values might vary slightly depending on the selected route.

```
BenchmarkAce_ParseStatic             38133007        30.89 ns/op         0 B/op        0 allocs/op
BenchmarkAero_ParseStatic            64270794        18.90 ns/op         0 B/op        0 allocs/op
BenchmarkBear_ParseStatic             7292257       163.2 ns/op       120 B/op        3 allocs/op
BenchmarkBeego_ParseStatic            2516884       480.0 ns/op       376 B/op        6 allocs/op
BenchmarkBone_ParseStatic             5892132       207.2 ns/op       144 B/op        3 allocs/op
BenchmarkChi_ParseStatic              4676241       264.3 ns/op       368 B/op        2 allocs/op
BenchmarkCloudyKitRouter_ParseStatic 83454457        14.33 ns/op         0 B/op        0 allocs/op
BenchmarkDenco_ParseStatic           50000000         42.6 ns/op         0 B/op        0 allocs/op
BenchmarkEcho_ParseStatic            44227891        25.76 ns/op         0 B/op        0 allocs/op
BenchmarkGin_ParseStatic             36790404        31.82 ns/op         0 B/op        0 allocs/op
BenchmarkGocraftWeb_ParseStatic       4825366       238.2 ns/op       288 B/op        5 allocs/op
BenchmarkGoji_ParseStatic            14832338        80.91 ns/op         0 B/op        0 allocs/op
BenchmarkGojiv2_ParseStatic          2021132       611.6 ns/op      1120 B/op        7 allocs/op
BenchmarkGoJsonRest_ParseStatic       3589072       344.3 ns/op       297 B/op       11 allocs/op
BenchmarkGoRestful_ParseStatic        510594      2613 ns/op      4408 B/op       14 allocs/op
BenchmarkGorillaMux_ParseStatic       1725338       687.4 ns/op       848 B/op        7 allocs/op
BenchmarkGowwwRouter_ParseStatic     66241981        17.56 ns/op         0 B/op        0 allocs/op
BenchmarkHttpRouter_ParseStatic      76173064        15.59 ns/op         0 B/op        0 allocs/op
BenchmarkHttpTreeMux_ParseStatic     43224706        28.14 ns/op         0 B/op        0 allocs/op
BenchmarkJin_ParseStatic              4734084       275.7 ns/op        64 B/op        2 allocs/op
BenchmarkKocha_ParseStatic            87662906        14.10 ns/op         0 B/op        0 allocs/op
BenchmarkLARS_ParseStatic            47451460        24.57 ns/op         0 B/op        0 allocs/op
BenchmarkMacaron_ParseStatic          1819621       649.5 ns/op       728 B/op        8 allocs/op
BenchmarkMartini_ParseStatic          835496      1728 ns/op       792 B/op       11 allocs/op
BenchmarkPat_ParseStatic              4293165       279.0 ns/op       240 B/op        5 allocs/op
BenchmarkPossum_ParseStatic           4066172       290.7 ns/op       416 B/op        3 allocs/op
BenchmarkR2router_ParseStatic        10886288       109.3 ns/op       112 B/op        3 allocs/op
BenchmarkRivet_ParseStatic            49660651        24.43 ns/op         0 B/op        0 allocs/op
BenchmarkTango_ParseStatic            3865370       318.7 ns/op       192 B/op        6 allocs/op
BenchmarkTigerTonic_ParseStatic      14127486        83.98 ns/op        48 B/op        1 allocs/op
BenchmarkTraffic_ParseStatic          1286270       918.5 ns/op      1224 B/op       18 allocs/op
BenchmarkVulcan_ParseStatic           7597268       159.7 ns/op        98 B/op        3 allocs/op

BenchmarkAce_ParseParam              15283216        77.76 ns/op        64 B/op        1 allocs/op
BenchmarkAero_ParseParam             53328355        23.61 ns/op         0 B/op        0 allocs/op
BenchmarkBear_ParseParam              3468222       328.2 ns/op       467 B/op        5 allocs/op
BenchmarkBeego_ParseParam             2259686       529.3 ns/op       400 B/op        6 allocs/op
BenchmarkBone_ParseParam              2102890       572.6 ns/op       832 B/op        6 allocs/op
BenchmarkChi_ParseParam               2639605       476.8 ns/op       704 B/op        4 allocs/op
BenchmarkCloudyKitRouter_ParseParam  62257131        17.55 ns/op         0 B/op        0 allocs/op
BenchmarkDenco_ParseParam            17717982        71.30 ns/op        64 B/op        1 allocs/op
BenchmarkEcho_ParseParam             37480790        31.57 ns/op         0 B/op        0 allocs/op
BenchmarkGin_ParseParam              31496392        39.37 ns/op         0 B/op        0 allocs/op
BenchmarkGocraftWeb_ParseParam        2709729       442.9 ns/op       656 B/op        8 allocs/op
BenchmarkGoji_ParseParam              4233016       279.5 ns/op       336 B/op        2 allocs/op
BenchmarkGojiv2_ParseParam            1729821       692.4 ns/op      1168 B/op        9 allocs/op
BenchmarkGoJsonRest_ParseParam        2264898       532.8 ns/op       617 B/op       13 allocs/op
BenchmarkGoRestful_ParseParam         438224      2818 ns/op      4728 B/op       15 allocs/op
BenchmarkGorillaMux_ParseParam       1413908       851.9 ns/op      1152 B/op        8 allocs/op
BenchmarkGowwwRouter_ParseParam       6467214       188.6 ns/op       368 B/op        2 allocs/op
BenchmarkHttpRouter_ParseParam       23831320        51.76 ns/op        64 B/op        1 allocs/op
BenchmarkHttpTreeMux_ParseParam       5659161       215.9 ns/op       352 B/op        3 allocs/op
BenchmarkKocha_ParseParam            13584864        90.21 ns/op        56 B/op        3 allocs/op
BenchmarkLARS_ParseParam             42559831        27.74 ns/op         0 B/op        0 allocs/op
BenchmarkMacaron_ParseParam           1255026       930.2 ns/op      1064 B/op       10 allocs/op
BenchmarkMartini_ParseParam           595198      1850 ns/op      1096 B/op       12 allocs/op
BenchmarkPat_ParseParam               1407506       852.0 ns/op       992 B/op       15 allocs/op
BenchmarkPossum_ParseParam            2994712       403.7 ns/op       496 B/op        5 allocs/op
BenchmarkR2router_ParseParam          5229079       228.4 ns/op       400 B/op        4 allocs/op
BenchmarkRivet_ParseParam            19251160        65.71 ns/op        48 B/op        1 allocs/op
BenchmarkTango_ParseParam             3436105       339.5 ns/op       224 B/op        6 allocs/op
BenchmarkTigerTonic_ParseParam        1341571       895.7 ns/op       752 B/op       15 allocs/op
BenchmarkTraffic_ParseParam            975212      1380 ns/op      1928 B/op       23 allocs/op
BenchmarkVulcan_ParseParam            5849358       196.6 ns/op        98 B/op        3 allocs/op

BenchmarkAce_Parse2Params             14570179        86.65 ns/op        64 B/op        1 allocs/op
BenchmarkAero_Parse2Params            40657292        29.98 ns/op         0 B/op        0 allocs/op
BenchmarkBear_Parse2Params             2993384       393.1 ns/op       496 B/op        5 allocs/op
BenchmarkBeego_Parse2Params            2148530       569.5 ns/op       424 B/op        6 allocs/op
BenchmarkBone_Parse2Params             2093529       549.3 ns/op       784 B/op        5 allocs/op
BenchmarkChi_Parse2Params              2466978       488.4 ns/op       704 B/op        4 allocs/op
BenchmarkCloudyKitRouter_Parse2Params 47855475        25.23 ns/op         0 B/op        0 allocs/op
BenchmarkDenco_Parse2Params           14628990        86.27 ns/op        64 B/op        1 allocs/op
BenchmarkEcho_Parse2Params            30394366        38.65 ns/op         0 B/op        0 allocs/op
BenchmarkGin_Parse2Params             23454494        53.09 ns/op         0 B/op        0 allocs/op
BenchmarkGocraftWeb_Parse2Params       2279914       512.3 ns/op       704 B/op        9 allocs/op
BenchmarkGoji_Parse2Params             4136914       286.0 ns/op       336 B/op        2 allocs/op
BenchmarkGojiv2_Parse2Params           1676466       696.5 ns/op      1152 B/op        8 allocs/op
BenchmarkGoJsonRest_Parse2Params       1868352       636.4 ns/op       681 B/op       14 allocs/op
BenchmarkGoRestful_Parse2Params         411751      3035 ns/op      5152 B/op       15 allocs/op
BenchmarkGorillaMux_Parse2Params     1000000      1058 ns/op      1168 B/op        8 allocs/op
BenchmarkGowwwRouter_Parse2Params     5627186       215.7 ns/op       368 B/op        2 allocs/op
BenchmarkHttpRouter_Parse2Params      13805226        78.40 ns/op        64 B/op        1 allocs/op
BenchmarkHttpTreeMux_Parse2Params      4096705       278.9 ns/op       384 B/op        4 allocs/op
BenchmarkJin_Parse2Params              4430271       307.4 ns/op        64 B/op        2 allocs/op
BenchmarkKocha_Parse2Params             6907125       181.0 ns/op       128 B/op        5 allocs/op
BenchmarkLARS_Parse2Params            33421621        35.13 ns/op         0 B/op        0 allocs/op
BenchmarkMacaron_Parse2Params          1214012       988.6 ns/op      1064 B/op       10 allocs/op
BenchmarkMartini_Parse2Params           667036      2022 ns/op      1176 B/op       13 allocs/op
BenchmarkPat_Parse2Params               1267580       920.6 ns/op       768 B/op       17 allocs/op
BenchmarkPossum_Parse2Params            2890536       403.1 ns/op       496 B/op        5 allocs/op
BenchmarkR2router_Parse2Params          4606222       242.0 ns/op       400 B/op        4 allocs/op
BenchmarkRivet_Parse2Params           12862880        94.64 ns/op        96 B/op        1 allocs/op
BenchmarkTango_Parse2Params             3200316       369.3 ns/op       264 B/op        6 allocs/op
BenchmarkTigerTonic_Parse2Params      1000000      1450 ns/op      1120 B/op       21 allocs/op
BenchmarkTraffic_Parse2Params           786621      1686 ns/op      1992 B/op       25 allocs/op
BenchmarkVulcan_Parse2Params           4512896       268.0 ns/op        98 B/op        3 allocs/op

BenchmarkAce_ParseAll                 1000000      1672 ns/op       640 B/op       16 allocs/op
BenchmarkAero_ParseAll                1907798       634.4 ns/op         0 B/op        0 allocs/op
BenchmarkBear_ParseAll                  152484      7582 ns/op      8920 B/op      110 allocs/op
BenchmarkBeego_ParseAll                 109524     11484 ns/op      9680 B/op      105 allocs/op
BenchmarkBone_ParseAll                   81542     14469 ns/op     15184 B/op      131 allocs/op
BenchmarkChi_ParseAll                   111600     10851 ns/op     14944 B/op       84 allocs/op
BenchmarkCloudyKitRouter_ParseAll     2617148       466.2 ns/op         0 B/op        0 allocs/op
BenchmarkDenco_ParseAll               1000000      1504 ns/op       928 B/op       16 allocs/op
BenchmarkEcho_ParseAll                 1333774       889.3 ns/op         0 B/op        0 allocs/op
BenchmarkGin_ParseAll                  1000000      1121 ns/op         0 B/op        0 allocs/op
BenchmarkGocraftWeb_ParseAll             95756     10915 ns/op     13520 B/op      181 allocs/op
BenchmarkGoji_ParseAll                  201721      5786 ns/op      5376 B/op       32 allocs/op
BenchmarkGojiv2_ParseAll                 65918     18071 ns/op     29456 B/op      199 allocs/op
BenchmarkGoJsonRest_ParseAll             78864     13471 ns/op     13034 B/op      321 allocs/op
BenchmarkGoRestful_ParseAll              16064     73970 ns/op    122320 B/op      380 allocs/op
BenchmarkGorillaMux_ParseAll             36490     33149 ns/op     26960 B/op      198 allocs/op
BenchmarkGowwwRouter_ParseAll           364011      3684 ns/op      5888 B/op       32 allocs/op
BenchmarkHttpRouter_ParseAll           1000000      1063 ns/op       640 B/op       16 allocs/op
BenchmarkHttpTreeMux_ParseAll           283706      4480 ns/op      5728 B/op       51 allocs/op
BenchmarkJin_ParseAll                    155251      7597 ns/op      1664 B/op       52 allocs/op
BenchmarkKocha_ParseAll                  735969      2191 ns/op      1112 B/op       54 allocs/op
BenchmarkLARS_ParseAll                  1618983       753.0 ns/op         0 B/op        0 allocs/op
BenchmarkMacaron_ParseAll                69536     17427 ns/op     18928 B/op      208 allocs/op
BenchmarkMartini_ParseAll                22546     54307 ns/op     25696 B/op      305 allocs/op
BenchmarkPat_ParseAll                    60537     20930 ns/op     15376 B/op      323 allocs/op
BenchmarkPossum_ParseAll                 146080      8418 ns/op     10816 B/op       78 allocs/op
BenchmarkR2router_ParseAll               210114      5625 ns/op      7520 B/op       94 allocs/op
BenchmarkRivet_ParseAll                1000000      1592 ns/op       912 B/op       16 allocs/op
BenchmarkTango_ParseAll                  133609      9505 ns/op      5864 B/op      156 allocs/op
BenchmarkTigerTonic_ParseAll              56746     20593 ns/op     15488 B/op      329 allocs/op
BenchmarkTraffic_ParseAll                 28068     40990 ns/op     45760 B/op      630 allocs/op
BenchmarkVulcan_ParseAll                  157276      7349 ns/op      2548 B/op       78 allocs/op
```


### [GitHub](http://developer.github.com/v3/)

The GitHub API is rather large, consisting of 203 routes. The tasks are basically the same as in the benchmarks before.

```
BenchmarkAce_GithubStatic            33030734        37.95 ns/op         0 B/op        0 allocs/op
BenchmarkAero_GithubStatic           73802552        15.94 ns/op         0 B/op        0 allocs/op
BenchmarkBear_GithubStatic             8067573       147.3 ns/op       120 B/op        3 allocs/op
BenchmarkBeego_GithubStatic            2352604       500.4 ns/op       400 B/op        6 allocs/op
BenchmarkBone_GithubStatic              303864      4008 ns/op      2880 B/op       60 allocs/op
BenchmarkChi_GithubStatic              4335577       277.6 ns/op       368 B/op        2 allocs/op
BenchmarkCloudyKitRouter_GithubStatic 63898783        19.70 ns/op         0 B/op        0 allocs/op
BenchmarkDenco_GithubStatic           82554795        14.67 ns/op         0 B/op        0 allocs/op
BenchmarkEcho_GithubStatic             34220596        35.41 ns/op         0 B/op        0 allocs/op
BenchmarkGin_GithubStatic             30923132        39.37 ns/op         0 B/op        0 allocs/op
BenchmarkGocraftWeb_GithubStatic       4907702       237.9 ns/op       288 B/op        5 allocs/op
BenchmarkGoji_GithubStatic            13124578        89.78 ns/op         0 B/op        0 allocs/op
BenchmarkGojiv2_GithubStatic           1980681       600.9 ns/op      1120 B/op        7 allocs/op
BenchmarkGoJsonRest_GithubStatic       3195270       385.3 ns/op       297 B/op       11 allocs/op
BenchmarkGoRestful_GithubStatic         346566      3787 ns/op      4408 B/op       14 allocs/op
BenchmarkGorillaMux_GithubStatic        787462      1734 ns/op       848 B/op        7 allocs/op
BenchmarkGowwwRouter_GithubStatic     31662435        37.46 ns/op         0 B/op        0 allocs/op
BenchmarkHttpRouter_GithubStatic      54140140        24.76 ns/op         0 B/op        0 allocs/op
BenchmarkHttpTreeMux_GithubStatic     42235080        27.81 ns/op         0 B/op        0 allocs/op
BenchmarkJin_GithubStatic              4481953       279.6 ns/op        64 B/op        2 allocs/op
BenchmarkKocha_GithubStatic           57859488        20.98 ns/op         0 B/op        0 allocs/op
BenchmarkLARS_GithubStatic             38953070        30.61 ns/op         0 B/op        0 allocs/op
BenchmarkMacaron_GithubStatic          1836092       645.9 ns/op       728 B/op        8 allocs/op
BenchmarkMartini_GithubStatic           438891      3138 ns/op       792 B/op       11 allocs/op
BenchmarkPat_GithubStatic               289694      4179 ns/op      3648 B/op       76 allocs/op
BenchmarkPossum_GithubStatic            4219516       284.9 ns/op       416 B/op        3 allocs/op
BenchmarkR2router_GithubStatic        10548290       111.4 ns/op       112 B/op        3 allocs/op
BenchmarkRivet_GithubStatic            34050860        35.31 ns/op         0 B/op        0 allocs/op
BenchmarkTango_GithubStatic             2714529       442.9 ns/op       192 B/op        6 allocs/op
BenchmarkTigerTonic_GithubStatic      14546265        81.57 ns/op        48 B/op        1 allocs/op
BenchmarkTraffic_GithubStatic           319030      4072 ns/op      4632 B/op       89 allocs/op
BenchmarkVulcan_GithubStatic            4388059       287.6 ns/op        98 B/op        3 allocs/op

BenchmarkAce_GithubParam               9829579       119.0 ns/op        96 B/op        1 allocs/op
BenchmarkAero_GithubParam             31295312        39.56 ns/op         0 B/op        0 allocs/op
BenchmarkBear_GithubParam               3015104       397.8 ns/op       496 B/op        5 allocs/op
BenchmarkBeego_GithubParam              1895560       645.3 ns/op       544 B/op        7 allocs/op
BenchmarkBone_GithubParam                421866      2986 ns/op      1824 B/op       18 allocs/op
BenchmarkChi_GithubParam                2151049       557.1 ns/op       704 B/op        4 allocs/op
BenchmarkCloudyKitRouter_GithubParam  20732406        56.72 ns/op         0 B/op        0 allocs/op
BenchmarkDenco_GithubParam              9875892       116.2 ns/op       128 B/op        1 allocs/op
BenchmarkEcho_GithubParam             18515860        64.34 ns/op         0 B/op        0 allocs/op
BenchmarkGin_GithubParam              15409566        77.03 ns/op         0 B/op        0 allocs/op
BenchmarkGocraftWeb_GithubParam        2403441       525.8 ns/op       704 B/op        9 allocs/op
BenchmarkGoji_GithubParam               3020839       395.9 ns/op       336 B/op        2 allocs/op
BenchmarkGojiv2_GithubParam             1404759       832.3 ns/op      1216 B/op       10 allocs/op
BenchmarkGoJsonRest_GithubParam        1589367       741.6 ns/op       681 B/op       14 allocs/op
BenchmarkGoRestful_GithubParam          260259      4748 ns/op      4408 B/op       15 allocs/op
BenchmarkGorillaMux_GithubParam         443834      2991 ns/op      1168 B/op        8 allocs/op
BenchmarkGowwwRouter_GithubParam       5368860       227.5 ns/op       368 B/op        2 allocs/op
BenchmarkHttpRouter_GithubParam      13169720        93.65 ns/op        96 B/op        1 allocs/op
BenchmarkHttpTreeMux_GithubParam       3926176       296.4 ns/op       384 B/op        4 allocs/op
BenchmarkJin_GithubParam                3990360       298.7 ns/op        64 B/op        2 allocs/op
BenchmarkKocha_GithubParam              6686547       181.5 ns/op       128 B/op        5 allocs/op
BenchmarkLARS_GithubParam              23532225        51.75 ns/op         0 B/op        0 allocs/op
BenchmarkMacaron_GithubParam          1000000      1021 ns/op      1064 B/op       10 allocs/op
BenchmarkMartini_GithubParam            311448      3875 ns/op      1176 B/op       13 allocs/op
BenchmarkPat_GithubParam                384381      3199 ns/op      2360 B/op       45 allocs/op
BenchmarkPossum_GithubParam             2968796       402.6 ns/op       496 B/op        5 allocs/op
BenchmarkR2router_GithubParam           4784548       242.7 ns/op       400 B/op        4 allocs/op
BenchmarkRivet_GithubParam               9125870       133.9 ns/op        96 B/op        1 allocs/op
BenchmarkTango_GithubParam               2371390       504.0 ns/op       296 B/op        6 allocs/op
BenchmarkTigerTonic_GithubParam        1000000      1340 ns/op      1072 B/op       21 allocs/op
BenchmarkTraffic_GithubParam             317230      3714 ns/op      2840 B/op       43 allocs/op
BenchmarkVulcan_GithubParam              2559393       465.0 ns/op        98 B/op        3 allocs/op

BenchmarkAce_GithubAll                   58052     20751 ns/op     13792 B/op      167 allocs/op
BenchmarkAero_GithubAll                 177111      6806 ns/op         0 B/op        0 allocs/op
BenchmarkBear_GithubAll                  14068     80618 ns/op     86448 B/op      943 allocs/op
BenchmarkBeego_GithubAll                 10000    120418 ns/op     85025 B/op     1036 allocs/op
BenchmarkBone_GithubAll                    1035   1072506 ns/op    709474 B/op     8453 allocs/op
BenchmarkChi_GithubAll                   10000    109933 ns/op    130817 B/op      740 allocs/op
BenchmarkCloudyKitRouter_GithubAll      128757      8778 ns/op         0 B/op        0 allocs/op
BenchmarkDenco_GithubAll                 51835     22025 ns/op     20224 B/op      167 allocs/op
BenchmarkEcho_GithubAll                  93256     12953 ns/op         0 B/op        0 allocs/op
BenchmarkGin_GithubAll                   93277     12939 ns/op         0 B/op        0 allocs/op
BenchmarkGocraftWeb_GithubAll            10000    105156 ns/op    130032 B/op     1686 allocs/op
BenchmarkGoji_GithubAll                   8091    172472 ns/op     56112 B/op      334 allocs/op
BenchmarkGojiv2_GithubAll                  4394    270208 ns/op    313744 B/op     3712 allocs/op
BenchmarkGoJsonRest_GithubAll              8544    144257 ns/op    127875 B/op     2737 allocs/op
BenchmarkGoRestful_GithubAll               1203    989962 ns/op    937306 B/op     3009 allocs/op
BenchmarkGorillaMux_GithubAll               970   1236556 ns/op    225666 B/op     1588 allocs/op
BenchmarkGowwwRouter_GithubAll            27093     44904 ns/op     61456 B/op      334 allocs/op
BenchmarkHttpRouter_GithubAll             79066     15619 ns/op     13792 B/op      167 allocs/op
BenchmarkHttpTreeMux_GithubAll            21541     55726 ns/op     65856 B/op      671 allocs/op
BenchmarkJin_GithubAll                    19173     63155 ns/op     12992 B/op      406 allocs/op
BenchmarkKocha_GithubAll                  31402     37613 ns/op     23304 B/op      843 allocs/op
BenchmarkLARS_GithubAll                  129565      8859 ns/op         0 B/op        0 allocs/op
BenchmarkMacaron_GithubAll                8918    138170 ns/op    147784 B/op     1624 allocs/op
BenchmarkMartini_GithubAll                 886   1374580 ns/op    231419 B/op     2731 allocs/op
BenchmarkPat_GithubAll                     759   1553106 ns/op   1421796 B/op    23019 allocs/op
BenchmarkPossum_GithubAll                 19530     60509 ns/op     84448 B/op      609 allocs/op
BenchmarkR2router_GithubAll               22016     51373 ns/op     70832 B/op      776 allocs/op
BenchmarkRivet_GithubAll                  48656     25189 ns/op     16272 B/op      167 allocs/op
BenchmarkTango_GithubAll                  10000    103984 ns/op     53854 B/op     1215 allocs/op
BenchmarkTigerTonic_GithubAll              4869    268895 ns/op    188584 B/op     4300 allocs/op
BenchmarkTraffic_GithubAll                  990   1220964 ns/op    829178 B/op    14582 allocs/op
BenchmarkVulcan_GithubAll                 15896     75923 ns/op     19894 B/op      609 allocs/op
```

### [Google+](https://developers.google.com/+/api/latest/)

Last but not least the Google+ API, consisting of 13 routes. In reality this is just a subset of a much larger API.

```
BenchmarkAce_GPlusStatic             41947334        28.21 ns/op         0 B/op        0 allocs/op
BenchmarkAero_GPlusStatic             72494410        17.40 ns/op         0 B/op        0 allocs/op
BenchmarkBear_GPlusStatic              8593870       126.6 ns/op       104 B/op        3 allocs/op
BenchmarkBeego_GPlusStatic             2327012       470.7 ns/op       376 B/op        6 allocs/op
BenchmarkBone_GPlusStatic             19505756        61.66 ns/op        32 B/op        1 allocs/op
BenchmarkChi_GPlusStatic              4473889       259.3 ns/op       368 B/op        2 allocs/op
BenchmarkCloudyKitRouter_GPlusStatic  82344060        14.36 ns/op         0 B/op        0 allocs/op
BenchmarkDenco_GPlusStatic            90577658        13.80 ns/op         0 B/op        0 allocs/op
BenchmarkEcho_GPlusStatic             46155442        25.14 ns/op         0 B/op        0 allocs/op
BenchmarkGin_GPlusStatic              42819369        27.69 ns/op         0 B/op        0 allocs/op
BenchmarkGocraftWeb_GPlusStatic        5217157       221.3 ns/op       272 B/op        5 allocs/op
BenchmarkGoji_GPlusStatic             19815026        59.99 ns/op         0 B/op        0 allocs/op
BenchmarkGojiv2_GPlusStatic            1988278       590.1 ns/op      1120 B/op        7 allocs/op
BenchmarkGoJsonRest_GPlusStatic        3888188       309.8 ns/op       297 B/op       11 allocs/op
BenchmarkGoRestful_GPlusStatic          590518      2132 ns/op      3984 B/op       14 allocs/op
BenchmarkGorillaMux_GPlusStatic        2060884       585.3 ns/op       848 B/op        7 allocs/op
BenchmarkGowwwRouter_GPlusStatic      75552476        16.28 ns/op         0 B/op        0 allocs/op
BenchmarkHttpRouter_GPlusStatic       81364206        14.12 ns/op         0 B/op        0 allocs/op
BenchmarkHttpTreeMux_GPlusStatic      66046198        18.28 ns/op         0 B/op        0 allocs/op
BenchmarkJin_GPlusStatic               4613403       273.9 ns/op        64 B/op        2 allocs/op
BenchmarkKocha_GPlusStatic            94489676        13.15 ns/op         0 B/op        0 allocs/op
BenchmarkLARS_GPlusStatic             49340887        23.71 ns/op         0 B/op        0 allocs/op
BenchmarkMacaron_GPlusStatic           1810120       663.3 ns/op       728 B/op        8 allocs/op
BenchmarkMartini_GPlusStatic            928950      1636 ns/op       792 B/op       11 allocs/op
BenchmarkPat_GPlusStatic              11213622       108.1 ns/op        96 B/op        2 allocs/op
BenchmarkPossum_GPlusStatic            4140832       285.4 ns/op       416 B/op        3 allocs/op
BenchmarkR2router_GPlusStatic        12189224        99.43 ns/op       112 B/op        3 allocs/op
BenchmarkRivet_GPlusStatic            50744248        22.94 ns/op         0 B/op        0 allocs/op
BenchmarkTango_GPlusStatic             4088084       295.6 ns/op       152 B/op        6 allocs/op
BenchmarkTigerTonic_GPlusStatic       21104206        57.07 ns/op        32 B/op        1 allocs/op
BenchmarkTraffic_GPlusStatic           1547344       778.9 ns/op      1080 B/op       15 allocs/op
BenchmarkVulcan_GPlusStatic            8180644       146.3 ns/op        98 B/op        3 allocs/op

BenchmarkAce_GPlusParam              14330941        83.60 ns/op        64 B/op        1 allocs/op
BenchmarkAero_GPlusParam             45916844        26.53 ns/op         0 B/op        0 allocs/op
BenchmarkBear_GPlusParam              3690564       327.2 ns/op       472 B/op        5 allocs/op
BenchmarkBeego_GPlusParam             2071672       577.7 ns/op       448 B/op        6 allocs/op
BenchmarkBone_GPlusParam              2038774       522.4 ns/op       752 B/op        5 allocs/op
BenchmarkChi_GPlusParam               2645270       459.1 ns/op       704 B/op        4 allocs/op
BenchmarkCloudyKitRouter_GPlusParam  48625714        21.55 ns/op         0 B/op        0 allocs/op
BenchmarkDenco_GPlusParam            15219870        84.19 ns/op        64 B/op        1 allocs/op
BenchmarkEcho_GPlusParam              33226914        36.53 ns/op         0 B/op        0 allocs/op
BenchmarkGin_GPlusParam               23436721        50.32 ns/op         0 B/op        0 allocs/op
BenchmarkGocraftWeb_GPlusParam        2778853       432.4 ns/op       640 B/op        8 allocs/op
BenchmarkGoji_GPlusParam              4583563       263.3 ns/op       336 B/op        2 allocs/op
BenchmarkGojiv2_GPlusParam            1683648       722.2 ns/op      1136 B/op        8 allocs/op
BenchmarkGoJsonRest_GPlusParam        2187098       552.4 ns/op       617 B/op       13 allocs/op
BenchmarkGoRestful_GPlusParam          557965      2425 ns/op      4328 B/op       15 allocs/op
BenchmarkGorillaMux_GPlusParam       1000000      1123 ns/op      1152 B/op        8 allocs/op
BenchmarkGowwwRouter_GPlusParam       5854920       195.1 ns/op       368 B/op        2 allocs/op
BenchmarkHttpRouter_GPlusParam       22364776        59.25 ns/op        64 B/op        1 allocs/op
BenchmarkHttpTreeMux_GPlusParam       5413417       223.7 ns/op       352 B/op        3 allocs/op
BenchmarkKocha_GPlusParam            12257280       101.1 ns/op        56 B/op        3 allocs/op
BenchmarkLARS_GPlusParam             37685482        32.61 ns/op         0 B/op        0 allocs/op
BenchmarkMacaron_GPlusParam           1271223       948.3 ns/op      1064 B/op       10 allocs/op
BenchmarkMartini_GPlusParam            709173      1969 ns/op      1096 B/op       12 allocs/op
BenchmarkPat_GPlusParam               1835019       649.7 ns/op       600 B/op       12 allocs/op
BenchmarkPossum_GPlusParam            2743230       410.6 ns/op       496 B/op        5 allocs/op
BenchmarkR2router_GPlusParam          5291772       227.6 ns/op       400 B/op        4 allocs/op
BenchmarkRivet_GPlusParam            17702534        69.86 ns/op        48 B/op        1 allocs/op
BenchmarkTango_GPlusParam             3421650       357.5 ns/op       216 B/op        6 allocs/op
BenchmarkTigerTonic_GPlusParam        1233201       937.6 ns/op       824 B/op       16 allocs/op
BenchmarkTraffic_GPlusParam            798312      1647 ns/op      1904 B/op       23 allocs/op
BenchmarkVulcan_GPlusParam            4001344       306.7 ns/op        98 B/op        3 allocs/op

BenchmarkAce_GPlus2Params             12015523       101.6 ns/op        64 B/op        1 allocs/op
BenchmarkAero_GPlus2Params            28911063        41.41 ns/op         0 B/op        0 allocs/op
BenchmarkBear_GPlus2Params             2998699       395.6 ns/op       496 B/op        5 allocs/op
BenchmarkBeego_GPlus2Params            1788684       697.8 ns/op       608 B/op        7 allocs/op
BenchmarkBone_GPlus2Params              851044      1564 ns/op      1104 B/op        9 allocs/op
BenchmarkChi_GPlus2Params              2233216       529.2 ns/op       704 B/op        4 allocs/op
BenchmarkCloudyKitRouter_GPlus2Params 35633578        34.05 ns/op         0 B/op        0 allocs/op
BenchmarkDenco_GPlus2Params           11595423       125.4 ns/op        64 B/op        1 allocs/op
BenchmarkEcho_GPlus2Params            21744290        56.84 ns/op         0 B/op        0 allocs/op
BenchmarkGin_GPlus2Params             14234064        93.85 ns/op         0 B/op        0 allocs/op
BenchmarkGocraftWeb_GPlus2Params       1958929       519.0 ns/op       704 B/op        9 allocs/op
BenchmarkGoji_GPlus2Params             3161919       376.3 ns/op       336 B/op        2 allocs/op
BenchmarkGojiv2_GPlus2Params          1280728       965.3 ns/op      1216 B/op       11 allocs/op
BenchmarkGoJsonRest_GPlus2Params       1460764       789.5 ns/op       681 B/op       14 allocs/op
BenchmarkGoRestful_GPlus2Params         491857      2769 ns/op      4424 B/op       15 allocs/op
BenchmarkGorillaMux_GPlus2Params        603730      2297 ns/op      1168 B/op        8 allocs/op
BenchmarkGowwwRouter_GPlus2Params     5642761       215.8 ns/op       368 B/op        2 allocs/op
BenchmarkHttpRouter_GPlus2Params     16912221        73.48 ns/op        64 B/op        1 allocs/op
BenchmarkHttpTreeMux_GPlus2Params      4026860       305.1 ns/op       384 B/op        4 allocs/op
BenchmarkKocha_GPlus2Params            6797592       191.9 ns/op       128 B/op        5 allocs/op
BenchmarkLARS_GPlus2Params            26838912        48.36 ns/op         0 B/op        0 allocs/op
BenchmarkMacaron_GPlus2Params        1000000      1071 ns/op      1064 B/op       10 allocs/op
BenchmarkMartini_GPlus2Params           299230      4216 ns/op      1224 B/op       15 allocs/op
BenchmarkPat_GPlus2Params               402188      2563 ns/op      2136 B/op       31 allocs/op
BenchmarkPossum_GPlus2Params           2855562       440.2 ns/op       496 B/op        5 allocs/op
BenchmarkR2router_GPlus2Params        4759280       260.9 ns/op       400 B/op        4 allocs/op
BenchmarkRivet_GPlus2Params           11254996       112.2 ns/op        96 B/op        1 allocs/op
BenchmarkTango_GPlus2Params            2951905       402.3 ns/op       296 B/op        6 allocs/op
BenchmarkTigerTonic_GPlus2Params       954858      1452 ns/op      1152 B/op       21 allocs/op
BenchmarkTraffic_GPlus2Params          437935      3179 ns/op      2296 B/op       31 allocs/op
BenchmarkVulcan_GPlus2Params           3175264       363.6 ns/op        98 B/op        3 allocs/op

BenchmarkAce_GPlusAll                 1000000      1085 ns/op       640 B/op       11 allocs/op
BenchmarkAero_GPlusAll                3032926       384.9 ns/op         0 B/op        0 allocs/op
BenchmarkBear_GPlusAll                  276094      4475 ns/op      5488 B/op       61 allocs/op
BenchmarkBeego_GPlusAll                 165290      7404 ns/op      5800 B/op       76 allocs/op
BenchmarkBone_GPlusAll                  101480     11416 ns/op     11040 B/op       98 allocs/op
BenchmarkChi_GPlusAll                   206954      6073 ns/op      8480 B/op       48 allocs/op
BenchmarkCloudyKitRouter_GPlusAll     4130020       295.2 ns/op         0 B/op        0 allocs/op
BenchmarkDenco_GPlusAll               1000000      1052 ns/op       672 B/op       11 allocs/op
BenchmarkEcho_GPlusAll                 2231187       544.9 ns/op         0 B/op        0 allocs/op
BenchmarkGin_GPlusAll                  1807658       673.2 ns/op         0 B/op        0 allocs/op
BenchmarkGocraftWeb_GPlusAll            192754      5836 ns/op      7936 B/op      103 allocs/op
BenchmarkGoji_GPlusAll                  360188      3519 ns/op      3696 B/op       22 allocs/op
BenchmarkGojiv2_GPlusAll                123594      9797 ns/op     15120 B/op      115 allocs/op
BenchmarkGoJsonRest_GPlusAll             146410      7860 ns/op      7701 B/op      170 allocs/op
BenchmarkGoRestful_GPlusAll              36752     33323 ns/op     56784 B/op      193 allocs/op
BenchmarkGorillaMux_GPlusAll             71676     17336 ns/op     14448 B/op      102 allocs/op
BenchmarkGowwwRouter_GPlusAll           578714      2458 ns/op      4048 B/op       22 allocs/op
BenchmarkHttpRouter_GPlusAll          1562707       744.1 ns/op       640 B/op       11 allocs/op
BenchmarkHttpTreeMux_GPlusAll           440953      2878 ns/op      4032 B/op       38 allocs/op
BenchmarkKocha_GPlusAll                 943507      1636 ns/op       976 B/op       43 allocs/op
BenchmarkLARS_GPlusAll                 2781660       429.3 ns/op         0 B/op        0 allocs/op
BenchmarkMacaron_GPlusAll               139842      8872 ns/op      9464 B/op      104 allocs/op
BenchmarkMartini_GPlusAll                38257     31630 ns/op     14328 B/op      171 allocs/op
BenchmarkPat_GPlusAll                    64677     18925 ns/op     15272 B/op      269 allocs/op
BenchmarkPossum_GPlusAll                302373      4120 ns/op      5408 B/op       39 allocs/op
BenchmarkR2router_GPlusAll              458011      3099 ns/op      4624 B/op       50 allocs/op
BenchmarkRivet_GPlusAll                1000000      1071 ns/op       768 B/op       11 allocs/op
BenchmarkTango_GPlusAll                 261090      4768 ns/op      3008 B/op       78 allocs/op
BenchmarkTigerTonic_GPlusAll             81028     15131 ns/op     11160 B/op      237 allocs/op
BenchmarkTraffic_GPlusAll                43917     28122 ns/op     26616 B/op      366 allocs/op
BenchmarkVulcan_GPlusAll                281142      4091 ns/op      1274 B/op       39 allocs/op
```


## Conclusions
First of all, there is no reason to use net/http's default [ServeMux](http://golang.org/pkg/net/http/#ServeMux), which is very limited and does not have especially good performance. There are enough alternatives coming in every flavor, choose the one you like best.

Secondly, the broad range of functions of some of the frameworks comes at a high price in terms of performance. For example Martini has great flexibility, but very bad performance. Martini has the worst performance of all tested routers in a lot of the benchmarks. Beego seems to have some scalability problems and easily defeats Martini with even worse performance, when the number of parameters or routes is high. I really hope, that the routing of these packages can be optimized. I think the Go-ecosystem needs great feature-rich frameworks like these.

Last but not least, we have to determine the performance champion.

Denco and its predecessor Kocha-urlrouter seem to have great performance, but are not convenient to use as a router for the net/http package. A lot of extra work is necessary to use it as a http.Handler. [The README of Denco claims](https://github.com/naoina/denco/blob/b03dbb499269a597afd0db715d408ebba1329d04/README.md), that the package is not intended as a replacement for [http.ServeMux](http://golang.org/pkg/net/http/#ServeMux).

[Goji](https://github.com/zenazn/goji/) looks very decent. It has great performance while also having a great range of features, more than any other router / framework in the top group.

Currently no router can beat the performance of the [HttpRouter](https://github.com/julienschmidt/httprouter) package, which currently dominates nearly all benchmarks.

In the end, performance can not be the (only) criterion for choosing a router. Play around a bit with some of the routers, and choose the one you like best.

## Usage

If you'd like to run these benchmarks locally, you'll need to install the package first:

```bash
go get github.com/julienschmidt/go-http-routing-benchmark
```
This may take a while due to the large number of dependencies that need to be downloaded. Once that command has finished you can run the full set of benchmarks like this:

```bash
cd $GOPATH/src/github.com/julienschmidt/go-http-routing-benchmark
go test -bench=.
```

> **Note:** If you run the tests and it SIGQUIT's make the go test timeout longer (#44)
>
```
go test -timeout=2h -bench=.
```


You can bench specific frameworks only by using a regular expression as the value of the `bench` parameter:
```bash
go test -bench="Martini|Gin|HttpMux"
```
