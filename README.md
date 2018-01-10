# How-to-setup-a-go-profiling

# Objective
Help developers profile their go application

# Tools
1. FlameGraph
1. `github.com/uber/go-torch`
1. `pacaur -S graphviz`
1. `github.com/google/pprof`

# Code level
Please ensure following handler have been enabled by your application in http server.
```go
imports (
  ...
  "net/http/pprof"
  ...
)

func someFunction(){
  ...
	http.Handle("/debug/pprof/", http.HandlerFunc(pprof.Index))
	http.Handle("/debug/pprof/cmdline", http.HandlerFunc(pprof.Cmdline))
	http.Handle("/debug/pprof/profile", http.HandlerFunc(pprof.Profile))
	http.Handle("/debug/pprof/symbol", http.HandlerFunc(pprof.Symbol))
	http.Handle("/debug/pprof/trace", http.HandlerFunc(pprof.Trace))
  ...
}
```

# Native solution ( go 1.10 )
1. ```git clone https://github.com/brendangregg/FlameGraph.git```
1. ```cp flamegraph.pl /usr/local/bin```
1. `go tool pprof --seconds 25 http://localhost:9090/debug/pprof/profile?seconds=25`
1. `go get github.com/google/pprof`
1. `pacaur -S graphviz`
1. pprof -http=127.0.0.1:9000 /home/alpha/pprof/pprof.main.samples.cpu.001.pb.gz

# Installation
1. ```git clone https://github.com/brendangregg/FlameGraph.git```
1. ```cp flamegraph.pl /usr/local/bin```
1. ```go get -v github.com/uber/go-torch```

# Show api call map
Step 1
```sh
# It will capture incoming call in 25s
go tool pprof --seconds 25 http://localhost:9090/debug/pprof/profile
```
Step 2
```sh
# Use your fav stress test tool e.g. tsenart/vegeta
```

Step 3 
```sh
# Use web to output the call map
Fetching profile from http://localhost:9090/debug/pprof/profile?seconds=25
Please wait... (25s)
Saved profile in /Users/lihaoquan/pprof/pprof.localhost:9090.samples.cpu.014.pb.gz
Entering interactive mode (type "help" for commands)
(pprof) web
```

# Output
![Call map](https://i.imgur.com/ibhsK7a.png)
# Show flame graph
```sh
# It will capture 30s incoming call
# Then generate a torch.svg in your current dir
go-torch -u http://localhost:9090 -t 30
```
# Output
![Cpu map](https://i.imgur.com/6VtscMY.png)

# How to read
- x-axis means the cpu time
- y-axis means sequence of function call
- Read from bottom to top
- Flame color is meaningless

# Reference
1. http://cizixs.com/2017/09/11/profiling-golang-program
1. http://lihaoquan.me/2017/1/1/Profiling-and-Optimizing-Go-using-go-torch.html
1. https://stackoverflow.com/questions/30560859/cant-use-go-tool-pprof-with-an-existing-server
