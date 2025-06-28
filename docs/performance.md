# Performance

This page gives an overview, of what to currently expect in terms of performance from Nuitka.

## Pystone Results

The results are the top value from this kind of output, running pystone 1000 times and taking the minimal value. The idea is that the fastest run is most meaningful, and eliminates usage spikes.

```bash
echo "Uncompiled Python2"
for i in {1..100}; do BENCH=1 python2 tests/benchmarks/pystone.py ; done | sort -rn | head -n 1
python2 -m nuitka --lto=yes --pgo tests/benchmarks/pystone.py
echo "Compiled Python2"
for i in {1..100}; do BENCH=1 ./pystone.bin ; done | sort -n | head -rn 1

echo "Uncompiled Python3"
for i in {1..100}; do BENCH=1 python3 tests/benchmarks/pystone3.py ; done | sort -rn | head -n 1
python3 -m nuitka --lto=yes --pgo tests/benchmarks/pystone3.py
echo "Compiled Python3"
for i in {1..100}; do BENCH=1 ./pystone3.bin ; done | sort -rn | head -n 1
```

## Benchmark Results

| Python | Uncompiled | Compiled LTO | Compiled PGO |
|--------|------------|--------------|--------------|
| Debian Python 2.7 | 137497.87 (1.000) | 460995.20 (3.353) | 503681.91 (3.663) |
| Nuitka Python 2.7 | 144074.78 (1.048) | 479271.51 (3.486) | 511247.44 (3.718) |

## Performance Expectations

### Compilation Modes

=== "Acceleration Mode"

    In acceleration mode (default), Nuitka compiles your Python modules but keeps them running in the same Python environment. This provides:

    - **Moderate speed improvements** (1.2x - 3x typical)
    - **Full compatibility** with the Python ecosystem
    - **Easy debugging** with familiar tools
    - **No deployment complexity**

=== "Standalone Mode"

    Standalone mode creates self-contained executables:

    - **Similar performance** to acceleration mode
    - **Larger file sizes** due to included dependencies
    - **No Python installation required** on target machines
    - **Longer compilation times**

=== "Onefile Mode"

    Onefile mode creates single executable files:

    - **Slight performance overhead** during startup
    - **Convenient distribution** as single file
    - **Temporary file extraction** on first run
    - **Best for simple applications**

### Optimization Features

#### Link Time Optimization (LTO)

Using `--lto=yes` enables link-time optimization which can provide significant performance improvements:

- **3-4x performance gains** in computational code
- **Longer compilation times**
- **Better cross-module optimization**
- **Recommended for production builds**

```bash
python -m nuitka --lto=yes your_script.py
```

#### Profile Guided Optimization (PGO)

Using `--pgo` enables profile-guided optimization:

- **Additional 5-15% performance** over LTO
- **Requires running the program** during compilation
- **Best for CPU-intensive applications**
- **Longer compilation process**

```bash
python -m nuitka --pgo your_script.py
```

### Performance Factors

#### What Gets Faster

- **Mathematical computations**
- **Loop-heavy code**
- **Function calls**
- **Local variable access**
- **String operations**

#### What Stays Similar

- **I/O operations** (file, network)
- **Third-party C extensions** (NumPy, etc.)
- **System calls**
- **Database operations**

#### Compilation Overhead

| Mode | Compilation Time | Disk Space | Memory Usage |
|------|------------------|------------|--------------|
| Acceleration | Fast | Minimal | Low |
| Standalone | Medium | High | Medium |
| Onefile | Slow | Medium | High |

### Real-World Performance

For typical Python applications, you can expect:

- **Web applications**: 20-50% faster response times
- **Data processing**: 2-4x faster execution
- **Mathematical calculations**: 3-10x performance gains
- **Startup time**: Similar or slightly faster (except onefile)

### Benchmarking Your Code

To measure performance improvements in your specific use case:

1. **Create a baseline** with standard Python
2. **Compile with Nuitka** using appropriate flags
3. **Run multiple iterations** to account for variance
4. **Consider using profiling tools** like `cProfile`

```python
import time

def benchmark_function():
    start_time = time.time()
    # Your code here
    end_time = time.time()
    return end_time - start_time

# Run multiple times and take average
times = [benchmark_function() for _ in range(100)]
average_time = sum(times) / len(times)
```

!!! tip "Performance Tips"
    - Use `--lto=yes` for production builds
    - Consider `--pgo` for CPU-intensive applications
    - Profile your code to identify bottlenecks
    - Test on target deployment platforms
    - Measure actual performance rather than assuming gains
