# Tutorial: Setup and Build

This is basic steps if you have nothing installed, of course if you have any of the parts, just skip it.

## Setup

### Install Python

1. Download and install Python from https://www.python.org/downloads/windows

2. Select one of `Windows x86-64 web-based installer` (64 bits Python, recommended) or `x86 executable` (32 bits Python) installer.

3. Verify it's working using command:
   ```bash
   python --version
   ```

### Install Nuitka

1. Install Nuitka using pip:
   ```bash
   python -m pip install nuitka
   ```

2. Verify using command:
   ```bash
   python -m nuitka --version
   ```

## Write some code and test

### Create a folder for the Python code

1. Create a directory:
   ```bash
   mkdir HelloWorld
   ```

2. Make a Python file named **hello.py**:

```python
def talk(message):
    return "Talk " + message


def main():
    print(talk("Hello World"))


if __name__ == "__main__":
    main()
```

### Test your program

Do as you normally would. Running Nuitka on code that works incorrectly is not easier to debug.

```bash
python hello.py
```

---

## Build it using

```bash
python -m nuitka hello.py
```

!!! note "C Compiler Setup"
    This will prompt you to download a C caching tool (to speed up repeated compilation of generated C code) and a MinGW64 based C compiler, unless you have a suitable MSVC installed. Say `yes` to both those questions.

## Run it

Execute the `hello.exe` created near `hello.py`.

## Distribute

To distribute, build with `--mode=standalone` option, which will not output a single executable, but a whole folder. Copy the resulting `hello.dist` folder to the other machine and run it.

You may also try `--onefile` which does create a single file, but make sure that the mere standalone is working, before turning to it, as it will make the debugging only harder, e.g. in case of missing data files.

### Distribution examples

=== "Standalone Mode"

    ```bash
    python -m nuitka --mode=standalone hello.py
    ```

    This creates a `hello.dist` folder containing all dependencies. Copy this entire folder to distribute your application.

=== "Onefile Mode"

    ```bash
    python -m nuitka --mode=onefile hello.py
    ```

    This creates a single executable file that contains everything needed to run your application.

=== "App Bundle (macOS)"

    ```bash
    python -m nuitka --mode=app hello.py
    ```

    This creates a macOS application bundle (.app file).

!!! tip "Distribution Best Practices"
    - Always test standalone mode first before trying onefile
    - Include any data files your application needs using `--include-data-files`
    - Test on the target platform before distribution
    - Consider using `--include-package-data` for package resources
