# Nuitka User Manual

This page is the recommended first read when you start using **Nuitka**.

This page will teach you more about **Nuitka** fundamentals and is the recommended first read when you start using **Nuitka**.

## Requirements

To ensure smooth work of **Nuitka**, make sure to follow the system requirements that include the following components:

### C Compiler

You need a C compiler with support for **C11** or for older Python only, a **C++** compiler for **C++03** [^1].

=== "Windows"

    Use one of the following compilers:

    - **Visual Studio 2022** or higher. Use the default English language pack to enable **Nuitka** to filter away irrelevant outputs and, therefore, have the best results.
      
      Using `--msvc=latest` enforces using this compiler.

    - The **MinGW64** compiler (used with `--mingw64`) option, must be the one Nuitka downloads, and it enforces that because there were frequent breakage with the complete tooling used.
      
      Nuitka will offer to download it unless it finds Visual Studio. Using `--mingw64` enforces using this compiler.

    - The **Clang-cl** compiler can be used if provided by the **Visual Studio** installer or Using `--clang` on Windows enforces the one from Visual Studio.

    - The **Clang** compiler can be used from from the **MinGW64** download.
      
      Using `--mingw64 --clang` enforces using this **Clang**.

=== "Linux"

    Use either the **GCC** from the system or an installed **clang**.

=== "macOS"

    Use the system **Clang** compiler. Install XCode via Apple Store to be covered.

=== "FreeBSD"

    On most architectures, use **Clang** or **GCC**, ideally matching the system compiler.

=== "Android"

    Install the required packages using:
    ```bash
    pkg install patchelf ccache binutils ldd termux-elf-cleaner
    ```

=== "Other Platforms"

    Use the **GCC** compiler of at least version 5.1 or higher. Use back-ports such as EPEL or SCL.

[^1]: Support for this **C11** is given with **GCC 5.1** or higher and all **Clang** versions. The older **MSVC** compilers don't do it yet. But as a workaround, with Python 3.10 or older, the **C++03** language standard is significantly enough overlapping with **C11**, such that it is then used instead.

### Python

**Python 2.7, 3.4, 3.5, 3.6, 3.7, 3.8, 3.9, 3.10, 3.11, 3.12, 3.13** are supported. If a stable Python release isn't listed here, don't worry; it's being worked on and added as soon as possible.

!!! warning "Special cases need 2 Python installations"
    In some scenarios, you might need to download an additional Python version. To get to know more, read the following statements:

    - If you use **Python 3.4**, additionally install either **Python 2** or **Python 3.5+**. You will need it during the compile time because **SCons** (which orchestrates the C compilation) does not support **Python 3.4**, but **Nuitka** does for some commercial users targeting Windows XP.

    - If you use the **Windows** opening system, don't use **Python 2** because **clcache** doesn't work with it. Install **Python 3.5+** instead. **Nuitka** finds these needed Python versions (for example, on **Windows** via registry); you shouldn't notice that as long as they are installed.

    - Other functionality is only available when another **Python** installs a specific package. For example, **Python2.x** can only achieve onefile compression if another **Python3** with the **zstandard** package is available.

!!! info "Important considerations"
    - **Moving binaries to other machines:** The created binaries can be made executable independent of the Python installation, with `--mode=standalone`, `--mode=onefile`, or `--mode=app` options, but not with `--mode=accelerated`.

    - **Binary filename suffix:** The created binaries have an `.exe` suffix on Windows. On other platforms, they have either no suffix in standalone mode or the `.bin` suffix, which you can remove or change with the `--output-filename` option. **Nuitka** adds the suffix for onefile and acceleration mode to make sure that the original script name and the binary name cannot ever collide, so we can safely overwrite the binary without destroying the source file.

    - **Module mode filenames:** Python Extension modules cannot be renamed without breaking them, the filename and the module name have to match, so you must change the source filename to get the desired result.

    - **It has to be a standard Python implementation:** You need a form of standard Python implementation, called **CPython**, to execute **Nuitka** because it's closely tied to the implementation details of it. Ideally, you use the [official Python](https://python.org) only.

    - **Homebrew (for macOS) is supported, but not ideal:** The resulting binaries are not as portable and specifically not backward portable.

    - **Anaconda Python is supported:** The Anaconda distribution is making special adaptations for some `conda` packages that lead to errors and might have to be reported as issues, such that special treatment can needed, but we add them as soon as they are reported.

    - **Python from Microsoft Store**: Don't download Python from **Microsoft Store**, as it doesn't work properly.

    - **Pyenv on macOS:** It is known that **macOS** **pyenv** does not work. Use **Homebrew** instead for self-compiled Python installations.

### Operating System

**Nuitka** supports the following operating systems: **Android**, **Linux**, **FreeBSD**, **NetBSD**, **OpenBSD**, **macOS**, and **Windows** (32 bits/64 bits/ARM).

The portability of the generated code is excellent. Therefore, other operating systems will work as well.

However, specific adjustments might be necessary, such as modifying Nuitka's internal **SCons** usage or providing additional flags. Ensure that the Python version matches the architecture of the C compiler, or else you will get cryptic error messages.

### Architecture

Supported Architectures are **x86**, **x86_64** (**AMD64**), and **ARM**.

**Nuitka** generally does not use any hardware specifics and produces portable C code. Therefore, many other architectures work out of the box as well.

Generally, the architectures that **Debian** or **RHEL** support can be considered good and tested, too; for example, **RISC-V** won't pose any issues.

## Installation

For most systems, there will be packages on the [Download page](download.md) of **Nuitka**. You can also install it from the source code via the standard `python setup.py install` routine or even run it directly from the source without installation.

## Command Line Usage

The recommended way of executing Nuitka is `<the_right_python> -m nuitka` to be absolutely certain which Python interpreter you are using, so it is easier to match with what Nuitka has.

The next best way of executing Nuitka bare that is from a source checkout or archive, with no environment variable changes, most noteworthy, you do not have to mess with `PYTHONPATH` at all for Nuitka. You just execute the `nuitka` and `nuitka-run` scripts directly without any changes to the environment. You may want to add the `bin` directory to your `PATH` for your convenience, but that step is optional.

Moreover, if you want to execute with the right interpreter, in that case, be sure to execute `<the_right_python> bin/nuitka` and be good.

### Pick the right Interpreter

If you encounter a `SyntaxError` you absolutely most certainly have picked the wrong interpreter for the program you are compiling.

Nuitka has a `--help` option to output what it can do:

```bash
python -m nuitka --help
```

The `nuitka-run` command is the same as `nuitka`, but with a different default. It tries to compile *and* directly execute a Python script:

```bash
python -m nuitka --run hello.py
```

This option that is different is `--run`, and passing on arguments after the first non-option to the created binary, so it is somewhat more similar to what plain `python` will do.

## License

Nuitka is licensed under the Apache License, Version 2.0; you may not use it except in compliance with the License.

You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
