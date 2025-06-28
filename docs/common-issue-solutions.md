# Solutions to Common Issues

We recommend this page if you find **Nuitka** is not working out of the box for you. There are typical issues encountered and their solutions.

This page can also teach you more about **Nuitka** advanced concepts; therefore, we recommend reading it for all levels of **Nuitka** users. It helps a lot to avoid issues and avoid non-optimal results.

## Deployment Mode

The non-deployment mode of **Nuitka** intends to assist you at the runtime of the compiled program. It aims to give you better error information and to catch typical user errors.

By default, **Nuitka** is set to compile in non-deployment mode, which can be deactivated using the `--deployment` option. In this mode, Nuitka activates a range of safety guards and helpers to identify and resolve any incorrect usage of **Nuitka**.

If you want to disable all these helpers, read more in the `Disabling All` section. Following is the list of currently implemented helpers you can also deactivate individually by name, and you might have to.

### Fork Bombs (Self-execution)

So after compilation, `sys.executable` is the compiled binary. Certain Python packages like `multiprocessing`, `joblib`, or `loky` typically expect to run from a full `Python` environment with `sys.executable`. They expect to use the `-c command` or `-m module_name` options to be able to launch other code temporarily or permanently as a service daemon.

However, with **Nuitka**, this executes your program again and puts these arguments in `sys.argv` where you may ignore them, and then you fork yourself again to launch the helper daemons. That leads to unintentionally repeated forking, potentially resulting in a scenario called **fork bomb**, where multiple processes spawn recursively, causing system freeze.

To avoid this issue, ensure your program handles command line parsing correctly and avoids using unsupported packages that attempt re-execution. Additionally, you can turn off this specific behavior at compile time by using the `--no-deployment-flag=self-execution` flag.

### Misleading Messages

Some Python packages generate misleading error messages when they encounter import failures. These messages sometimes suggest actions that may not be the appropriate solution for compiled programs. **Nuitka** tries to correct them in non-deployment mode.

Here is an example where **Nuitka** changes a message that asks to `pip install ...` a package. That, of course, is not the issue, and instead, **Nuitka** makes the program point the user to the `--include-module` option to use.

```yaml
- module-name: 'imageio.core.imopen'
  anti-bloat:
    - replacements_plain:
        '`pip install imageio[{config.install_name}]` to install it': '`--include-module={config.module_name}` with Nuitka to include it'
        'err_type = ImportError': 'err_type = RuntimeError'
      when: 'not deployment'
```

### And much more

The non-deployment mode of **Nuitka** is constantly evolving and adding more features, for example, something for `FileNotFoundError` should be added; there are plenty of ideas.

### Disabling All

Of course, all these helpers are removed at once when using the `--deployment` option of **Nuitka**, but remember that you may want to re-enable it for debugging. To make this easy to toggle, you should use [Nuitka Project Options](#) and check an environment variable in them.

```python
# nuitka-project-if: os.getenv("DEPLOYMENT") == "yes":
#  nuitka-project: --deployment
```

We recommend selective disabling, as with **PyPI** upgrades and your code changes, these issues can resurface. The space saved by deployment mode is minimal, so we advise not to disable them. Instead, review each feature, and if you know, it won't affect your software or you won't need it, turn it off.

## Windows Virus Scanners

Some Antivirus Vendors may flag compile binaries using **Nuitka's** default settings on **Windows** as malware. You can avoid this by purchasing the Nuitka Commercial plan and following the instructions given.

## Linux Standalone

For **Linux** standalone, building a binary that works on older **Linux** versions is challenging. The solution is to compile your application on the oldest **Linux** version that you intend to support.

We recommend purchasing Nuitka Commercial plan to overcome this issue without extra effort. **Nuitka Commercial** has container-based builds that you can use.

## Program Crashes System (Fork Bombs)

A fork bomb is a program that spawns recursively, causing a system lock and ultimately crashing it in short order. **Nuitka** handles it for all packages known to do that.

To debug fork bombs, use the `--experimental=debug-self-forking` option to check program fork behavior.

```python
import os, sys

if "NUITKA_LAUNCH_TOKEN" in os.environ:
   sys.exit("Error, launch token must not be present or else fork bomb suspected.")
os.environ["NUITKA_LAUNCH_TOKEN"] = "1"
```

## Memory issues and compiler bugs

Sometimes, the C compilers will crash with unspecific errors. It may be saying they cannot allocate memory, that some assembly input was truncated, or other similar error messages. All of these can be caused by using more memory than is available.

### Solutions

- **Ask Nuitka to use less memory:** There is a dedicated option `--low-memory` which influences decisions of Nuitka, such that it avoids high usage of memory during compilation at the cost of increased compile time.

- **Avoid 32-bit C compiler/assembler memory limits:** Do not use a 32 bits compiler, but a 64 bit one.

- **Use LTO compilation or not:** With `--lto=yes` or `--lto=no` you can switch the C compilation to handle memory differently.

- **Switch the C compiler to clang:** Clang may be more memory efficient than gcc on certain platforms.

- **Add a larger swap file to your embedded Linux:** Use swap space to handle larger memory requirements.

- **Limit the amount of compilation jobs:** Use the `--jobs` option of Nuitka to limit parallel compilations.
