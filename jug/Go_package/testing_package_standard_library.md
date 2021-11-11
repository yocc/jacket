https://pkg.go.dev/testing



### Overview [¶](https://pkg.go.dev/testing#pkg-overview)

- [Benchmarks](https://pkg.go.dev/testing#hdr-Benchmarks)
- [Examples](https://pkg.go.dev/testing#hdr-Examples)
- [Skipping](https://pkg.go.dev/testing#hdr-Skipping)
- [Subtests and Sub-benchmarks](https://pkg.go.dev/testing#hdr-Subtests_and_Sub_benchmarks)
- [Main](https://pkg.go.dev/testing#hdr-Main)

Package testing provides support for automated testing of Go packages. It is intended to be used in concert with the "go test" command, which automates execution of any function of the form

```
func TestXxx(*testing.T)
```

where Xxx does not start with a lowercase letter. The function name serves to identify the test routine.

Within these functions, use the Error, Fail or related methods to signal failure.

To write a new test suite, create a file whose name ends _test.go that contains the TestXxx functions as described here. Put the file in the same package as the one being tested. The file will be excluded from regular package builds but will be included when the "go test" command is run. For more detail, run "go help test" and "go help testflag".

A simple test function looks like this:

```
func TestAbs(t *testing.T) {
    got := Abs(-1)
    if got != 1 {
        t.Errorf("Abs(-1) = %d; want 1", got)
    }
}
```

#### Benchmarks [¶](https://pkg.go.dev/testing#hdr-Benchmarks)

Functions of the form

```
func BenchmarkXxx(*testing.B)
```

are considered benchmarks, and are executed by the "go test" command when its -bench flag is provided. Benchmarks are run sequentially.

For a description of the testing flags, see https://golang.org/cmd/go/#hdr-Testing_flags

A sample benchmark function looks like this:

```
func BenchmarkRandInt(b *testing.B) {
    for i := 0; i < b.N; i++ {
        rand.Int()
    }
}
```

The benchmark function must run the target code b.N times. During benchmark execution, b.N is adjusted until the benchmark function lasts long enough to be timed reliably. The output

```
BenchmarkRandInt-8   	68453040	        17.8 ns/op
```

means that the loop ran 68453040 times at a speed of 17.8 ns per loop.

If a benchmark needs some expensive setup before running, the timer may be reset:

```
func BenchmarkBigLen(b *testing.B) {
    big := NewBig()
    b.ResetTimer()
    for i := 0; i < b.N; i++ {
        big.Len()
    }
}
```

If a benchmark needs to test performance in a parallel setting, it may use the RunParallel helper function; such benchmarks are intended to be used with the go test -cpu flag:

```
func BenchmarkTemplateParallel(b *testing.B) {
    templ := template.Must(template.New("test").Parse("Hello, {{.}}!"))
    b.RunParallel(func(pb *testing.PB) {
        var buf bytes.Buffer
        for pb.Next() {
            buf.Reset()
            templ.Execute(&buf, "World")
        }
    })
}
```

#### Examples [¶](https://pkg.go.dev/testing#hdr-Examples)

The package also runs and verifies example code. Example functions may include a concluding line comment that begins with "Output:" and is compared with the standard output of the function when the tests are run. (The comparison ignores leading and trailing space.) These are examples of an example:

```
func ExampleHello() {
    fmt.Println("hello")
    // Output: hello
}

func ExampleSalutations() {
    fmt.Println("hello, and")
    fmt.Println("goodbye")
    // Output:
    // hello, and
    // goodbye
}
```

The comment prefix "Unordered output:" is like "Output:", but matches any line order:

```
func ExamplePerm() {
    for _, value := range Perm(5) {
        fmt.Println(value)
    }
    // Unordered output: 4
    // 2
    // 1
    // 3
    // 0
}
```

Example functions without output comments are compiled but not executed.

The naming convention to declare examples for the package, a function F, a type T and method M on type T are:

```
func Example() { ... }
func ExampleF() { ... }
func ExampleT() { ... }
func ExampleT_M() { ... }
```

Multiple example functions for a package/type/function/method may be provided by appending a distinct suffix to the name. The suffix must start with a lower-case letter.

```
func Example_suffix() { ... }
func ExampleF_suffix() { ... }
func ExampleT_suffix() { ... }
func ExampleT_M_suffix() { ... }
```

The entire test file is presented as the example when it contains a single example function, at least one other function, type, variable, or constant declaration, and no test or benchmark functions.

#### Skipping [¶](https://pkg.go.dev/testing#hdr-Skipping)

Tests or benchmarks may be skipped at run time with a call to the Skip method of *T or *B:

```
func TestTimeConsuming(t *testing.T) {
    if testing.Short() {
        t.Skip("skipping test in short mode.")
    }
    ...
}
```

#### Subtests and Sub-benchmarks [¶](https://pkg.go.dev/testing#hdr-Subtests_and_Sub_benchmarks)

The Run methods of T and B allow defining subtests and sub-benchmarks, without having to define separate functions for each. This enables uses like table-driven benchmarks and creating hierarchical tests. It also provides a way to share common setup and tear-down code:

```
func TestFoo(t *testing.T) {
    // <setup code>
    t.Run("A=1", func(t *testing.T) { ... })
    t.Run("A=2", func(t *testing.T) { ... })
    t.Run("B=1", func(t *testing.T) { ... })
    // <tear-down code>
}
```

Each subtest and sub-benchmark has a unique name: the combination of the name of the top-level test and the sequence of names passed to Run, separated by slashes, with an optional trailing sequence number for disambiguation.

The argument to the -run and -bench command-line flags is an unanchored regular expression that matches the test's name. For tests with multiple slash-separated elements, such as subtests, the argument is itself slash-separated, with expressions matching each name element in turn. Because it is unanchored, an empty expression matches any string. For example, using "matching" to mean "whose name contains":

```
go test -run ”      # Run all tests.
go test -run Foo     # Run top-level tests matching "Foo", such as "TestFooBar".
go test -run Foo/A=  # For top-level tests matching "Foo", run subtests matching "A=".
go test -run /A=1    # For all top-level tests, run subtests matching "A=1".
```

Subtests can also be used to control parallelism. A parent test will only complete once all of its subtests complete. In this example, all tests are run in parallel with each other, and only with each other, regardless of other top-level tests that may be defined:

```
func TestGroupedParallel(t *testing.T) {
    for _, tc := range tests {
        tc := tc // capture range variable
        t.Run(tc.Name, func(t *testing.T) {
            t.Parallel()
            ...
        })
    }
}
```

The race detector kills the program if it exceeds 8192 concurrent goroutines, so use care when running parallel tests with the -race flag set.

Run does not return until parallel subtests have completed, providing a way to clean up after a group of parallel tests:

```
func TestTeardownParallel(t *testing.T) {
    // This Run will not return until the parallel tests finish.
    t.Run("group", func(t *testing.T) {
        t.Run("Test1", parallelTest1)
        t.Run("Test2", parallelTest2)
        t.Run("Test3", parallelTest3)
    })
    // <tear-down code>
}
```

#### Main [¶](https://pkg.go.dev/testing#hdr-Main)

It is sometimes necessary for a test program to do extra setup or teardown before or after testing. It is also sometimes necessary for a test to control which code runs on the main thread. To support these and other cases, if a test file contains a function:

```
func TestMain(m *testing.M)
```

then the generated test will call TestMain(m) instead of running the tests directly. TestMain runs in the main goroutine and can do whatever setup and teardown is necessary around a call to m.Run. m.Run will return an exit code that may be passed to os.Exit. If TestMain returns, the test wrapper will pass the result of m.Run to os.Exit itself.

When TestMain is called, flag.Parse has not been run. If TestMain depends on command-line flags, including those of the testing package, it should call flag.Parse explicitly. Command line flags are always parsed by the time test or benchmark functions run.

A simple implementation of TestMain is:

```
func TestMain(m *testing.M) {
	// call flag.Parse() here if TestMain uses flags
	os.Exit(m.Run())
}
```

### Index [¶](https://pkg.go.dev/testing#pkg-index)

- [func AllocsPerRun(runs int, f func()) (avg float64)](https://pkg.go.dev/testing#AllocsPerRun)
- [func CoverMode() string](https://pkg.go.dev/testing#CoverMode)
- [func Coverage() float64](https://pkg.go.dev/testing#Coverage)
- [func Init()](https://pkg.go.dev/testing#Init)
- [func Main(matchString func(pat, str string) (bool, error), tests [\]InternalTest, ...)](https://pkg.go.dev/testing#Main)
- [func RegisterCover(c Cover)](https://pkg.go.dev/testing#RegisterCover)
- [func RunBenchmarks(matchString func(pat, str string) (bool, error), ...)](https://pkg.go.dev/testing#RunBenchmarks)
- [func RunExamples(matchString func(pat, str string) (bool, error), examples [\]InternalExample) (ok bool)](https://pkg.go.dev/testing#RunExamples)
- [func RunTests(matchString func(pat, str string) (bool, error), tests [\]InternalTest) (ok bool)](https://pkg.go.dev/testing#RunTests)
- [func Short() bool](https://pkg.go.dev/testing#Short)
- [func Verbose() bool](https://pkg.go.dev/testing#Verbose)
- [type B](https://pkg.go.dev/testing#B)
- - [func (c *B) Cleanup(f func())](https://pkg.go.dev/testing#B.Cleanup)
  - [func (c *B) Error(args ...interface{})](https://pkg.go.dev/testing#B.Error)
  - [func (c *B) Errorf(format string, args ...interface{})](https://pkg.go.dev/testing#B.Errorf)
  - [func (c *B) Fail()](https://pkg.go.dev/testing#B.Fail)
  - [func (c *B) FailNow()](https://pkg.go.dev/testing#B.FailNow)
  - [func (c *B) Failed() bool](https://pkg.go.dev/testing#B.Failed)
  - [func (c *B) Fatal(args ...interface{})](https://pkg.go.dev/testing#B.Fatal)
  - [func (c *B) Fatalf(format string, args ...interface{})](https://pkg.go.dev/testing#B.Fatalf)
  - [func (c *B) Helper()](https://pkg.go.dev/testing#B.Helper)
  - [func (c *B) Log(args ...interface{})](https://pkg.go.dev/testing#B.Log)
  - [func (c *B) Logf(format string, args ...interface{})](https://pkg.go.dev/testing#B.Logf)
  - [func (c *B) Name() string](https://pkg.go.dev/testing#B.Name)
  - [func (b *B) ReportAllocs()](https://pkg.go.dev/testing#B.ReportAllocs)
  - [func (b *B) ReportMetric(n float64, unit string)](https://pkg.go.dev/testing#B.ReportMetric)
  - [func (b *B) ResetTimer()](https://pkg.go.dev/testing#B.ResetTimer)
  - [func (b *B) Run(name string, f func(b *B)) bool](https://pkg.go.dev/testing#B.Run)
  - [func (b *B) RunParallel(body func(*PB))](https://pkg.go.dev/testing#B.RunParallel)
  - [func (b *B) SetBytes(n int64)](https://pkg.go.dev/testing#B.SetBytes)
  - [func (b *B) SetParallelism(p int)](https://pkg.go.dev/testing#B.SetParallelism)
  - [func (c *B) Skip(args ...interface{})](https://pkg.go.dev/testing#B.Skip)
  - [func (c *B) SkipNow()](https://pkg.go.dev/testing#B.SkipNow)
  - [func (c *B) Skipf(format string, args ...interface{})](https://pkg.go.dev/testing#B.Skipf)
  - [func (c *B) Skipped() bool](https://pkg.go.dev/testing#B.Skipped)
  - [func (b *B) StartTimer()](https://pkg.go.dev/testing#B.StartTimer)
  - [func (b *B) StopTimer()](https://pkg.go.dev/testing#B.StopTimer)
  - [func (c *B) TempDir() string](https://pkg.go.dev/testing#B.TempDir)
- [type BenchmarkResult](https://pkg.go.dev/testing#BenchmarkResult)
- - [func Benchmark(f func(b *B)) BenchmarkResult](https://pkg.go.dev/testing#Benchmark)
- - [func (r BenchmarkResult) AllocedBytesPerOp() int64](https://pkg.go.dev/testing#BenchmarkResult.AllocedBytesPerOp)
  - [func (r BenchmarkResult) AllocsPerOp() int64](https://pkg.go.dev/testing#BenchmarkResult.AllocsPerOp)
  - [func (r BenchmarkResult) MemString() string](https://pkg.go.dev/testing#BenchmarkResult.MemString)
  - [func (r BenchmarkResult) NsPerOp() int64](https://pkg.go.dev/testing#BenchmarkResult.NsPerOp)
  - [func (r BenchmarkResult) String() string](https://pkg.go.dev/testing#BenchmarkResult.String)
- [type Cover](https://pkg.go.dev/testing#Cover)
- [type CoverBlock](https://pkg.go.dev/testing#CoverBlock)
- [type InternalBenchmark](https://pkg.go.dev/testing#InternalBenchmark)
- [type InternalExample](https://pkg.go.dev/testing#InternalExample)
- [type InternalTest](https://pkg.go.dev/testing#InternalTest)
- [type M](https://pkg.go.dev/testing#M)
- - [func MainStart(deps testDeps, tests [\]InternalTest, benchmarks []InternalBenchmark, ...) *M](https://pkg.go.dev/testing#MainStart)
- - [func (m *M) Run() (code int)](https://pkg.go.dev/testing#M.Run)
- [type PB](https://pkg.go.dev/testing#PB)
- - [func (pb *PB) Next() bool](https://pkg.go.dev/testing#PB.Next)
- [type T](https://pkg.go.dev/testing#T)
- - [func (c *T) Cleanup(f func())](https://pkg.go.dev/testing#T.Cleanup)
  - [func (t *T) Deadline() (deadline time.Time, ok bool)](https://pkg.go.dev/testing#T.Deadline)
  - [func (c *T) Error(args ...interface{})](https://pkg.go.dev/testing#T.Error)
  - [func (c *T) Errorf(format string, args ...interface{})](https://pkg.go.dev/testing#T.Errorf)
  - [func (c *T) Fail()](https://pkg.go.dev/testing#T.Fail)
  - [func (c *T) FailNow()](https://pkg.go.dev/testing#T.FailNow)
  - [func (c *T) Failed() bool](https://pkg.go.dev/testing#T.Failed)
  - [func (c *T) Fatal(args ...interface{})](https://pkg.go.dev/testing#T.Fatal)
  - [func (c *T) Fatalf(format string, args ...interface{})](https://pkg.go.dev/testing#T.Fatalf)
  - [func (c *T) Helper()](https://pkg.go.dev/testing#T.Helper)
  - [func (c *T) Log(args ...interface{})](https://pkg.go.dev/testing#T.Log)
  - [func (c *T) Logf(format string, args ...interface{})](https://pkg.go.dev/testing#T.Logf)
  - [func (c *T) Name() string](https://pkg.go.dev/testing#T.Name)
  - [func (t *T) Parallel()](https://pkg.go.dev/testing#T.Parallel)
  - [func (t *T) Run(name string, f func(t *T)) bool](https://pkg.go.dev/testing#T.Run)
  - [func (c *T) Skip(args ...interface{})](https://pkg.go.dev/testing#T.Skip)
  - [func (c *T) SkipNow()](https://pkg.go.dev/testing#T.SkipNow)
  - [func (c *T) Skipf(format string, args ...interface{})](https://pkg.go.dev/testing#T.Skipf)
  - [func (c *T) Skipped() bool](https://pkg.go.dev/testing#T.Skipped)
  - [func (c *T) TempDir() string](https://pkg.go.dev/testing#T.TempDir)
- [type TB](https://pkg.go.dev/testing#TB)

#### Examples [¶](https://pkg.go.dev/testing#pkg-examples)

- [B.ReportMetric](https://pkg.go.dev/testing#example-B.ReportMetric)
- [B.RunParallel](https://pkg.go.dev/testing#example-B.RunParallel)

### Constants [¶](https://pkg.go.dev/testing#pkg-constants)

This section is empty.

### Variables [¶](https://pkg.go.dev/testing#pkg-variables)

This section is empty.