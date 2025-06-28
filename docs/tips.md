# Tips

On this page, you'll find helpful tips and techniques for optimizing your experience with Nuitka. From maximizing compilation efficiency to managing dependencies and runtime considerations.

## Nuitka Options in Code (Nuitka Project Options)

You can create a build script or directly track the Nuitka command line options inside the source code. The latter is a much cleaner approach than a build script that constructs a command line to invoke Nuitka and still very powerful.

In your build script, you use `python -m nuitka some_script.py --output-dir=dist` and put only options that are not generally relevant; in the main `script.py` you have lines like these.

```python
# The PySide6 plugin covers qt-plugins
# nuitka-project: --enable-plugin=pyside6
# nuitka-project: --include-qt-plugins=qml
```

You can have conditions, you can evaluate environment variables, you can use locations relative to the main script, and many more things.

## Passing Python Command Line Flags

For passing things like `-O` or `-S` to Python, to your compiled program, there is a command line option name `--python-flag=` which makes Nuitka emulate these options.

The most important ones are supported, and we will add more if a use case exists.

## Caching Compilation Results

When invoked with identical input files, the C compiler will take a long time and require a lot of CPU to compile over and over. Make sure to have `ccache` installed and configured when using gcc. It will make repeated compilations much faster, even if things are not yet perfect, i.e. changes to the program can cause many C files to change, requiring a new compilation instead of using the cached result.

=== "Windows"

    On **Windows**, with the included MinGW64 **Nuitka** supports using `ccache.exe` and will offer to download from an official source and use it automatically, and using that is recommended for Windows, as other versions are known to very often have severe quality issues; for example, they spuriously cause errors or hang indefinitely or don't work at all.

    For the MSVC compilers and ClangCL setups, using the `clcache` is automatic and included in Nuitka.

=== "macOS"

    On macOS and Intel, there is an automatic download of a `ccache` binary from our site. For arm64 arches, it's recommended to use this setup, which installs Homebrew and ccache. Nuitka picks it up automatically if it is on that kind of machine.

    ```bash
    export HOMEBREW_INSTALL_FROM_API=1
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
    eval $(/opt/homebrew/bin/brew shellenv)
    brew install ccache
    ```

=== "Linux/Other"

    Nuitka will pick up `ccache` if located in system `PATH`, and it will also be possible to provide if by setting `NUITKA_CCACHE_BINARY` to the full path of the binary. This is useful in CI systems where environments might be non-standard.

## Controlling Cache Storage Locations

The storage for cache results of all kinds, downloads, and cached compilation results from C and **Nuitka**, is done in a platform-dependent directory as determined by the `appdirs` package. However, you can override it by setting the environment variable `NUITKA_CACHE_DIR` to a base directory. This is for use in environments where the home directory does not persist but other paths do.

There is also per cache control of these caches. Here is a table of environment variables that you can set before starting the compilation, to make Nuitka store some of these caches in an entirely separate space.

| Cache name | Environment Variable | Data Put there |
|------------|---------------------|----------------|
| downloads | NUITKA_CACHE_DIR_DOWNLOADS | Downloads made, e.g., dependency walker |
| ccache | NUITKA_CACHE_DIR_CCACHE | Object files created by gcc |
| clcache | NUITKA_CACHE_DIR_CLCACHE | Object files created by MSVC |
| bytecode | NUITKA_CACHE_DIR_BYTECODE | Bytecode of demoted modules |
| dll-dependencies | NUITKA_CACHE_DIR_DLL_DEPENDENCIES | DLL dependencies |

## Using the Correct Nuitka Runner

Avoid running the `nuitka` binary; doing `python -m nuitka` will make sure that you are using what you think you are. Using the wrong Python will make it give you `SyntaxError` or `ImportError` for installed modules. That happens, when you run **Nuitka** with Python2 on Python3 code and vice versa. You avoid that issue by explicitly calling the same Python interpreter binary.

## Choosing the Fastest C Compilers

The fastest binaries of `pystone.exe` on Windows with 64 bits version of Python proved to be significantly faster with MinGW64, roughly 20% better score. So, it is recommended for use over MSVC. Using `clang-cl.exe` of Clang was faster than MSVC but still significantly slower than MinGW64 and will be harder to use, so it that's not recommended as well.

On Linux, for `pystone.bin`, the binary produced by `clang6` was faster than `gcc-6.3`, but not significantly. Since gcc is more often already installed, it is recommended for use for now.

Differences in C compilation times were not examined.

## Addressing Unexpected Slowdowns

Using the Python DLL, as standard CPython does, can lead to unexpected slowdowns, for example in uncompiled code that works with Unicode strings. This is because calling to the DLL rather than residing in the DLL causes overhead, and this even happens to the DLL with itself, being slower, than a Python all contained in one binary.

So if feasible, aim at static linking, which is currently only possible with Anaconda Python on non-Windows, Debian Python2, self compiled Pythons (do not activate `--enable-shared`, not needed), and installs created with `pyenv`.

!!! note "Anaconda Static Linking"
    On Anaconda, you may need to execute `conda install libpython-static`

## Managing Standalone Executables and Dependencies

The process of making standalone executables for Windows traditionally involves using an external dependency walker to copy necessary libraries along with the compiled executables to the distribution folder.

There are plenty of ways to find that something is missing. Do not manually copy things into the folder, esp. not DLLs, as that's not going to work. Instead, make bug reports to get these handled by Nuitka properly.

## Resolving Windows Resource Update Errors

On Windows, the Windows Defender tool and the Windows Indexing Service both scan the freshly created binaries, while Nuitka wants to work with it, e.g. adding more resources, and then preventing operations randomly due to holding locks. Make sure to exclude your compilation result directories from these services.

## Redistributing Windows Standalone Programs

Whether compiling with MingW or MSVC, the standalone programs have external dependencies to Visual C Runtime libraries. Nuitka tries to ship those dependent DLLs by copying them from your system.

Beginning with Microsoft Windows 10, Microsoft ships `ucrt.dll` (Universal C Runtime libraries) which handles calls to various C runtime functions.
