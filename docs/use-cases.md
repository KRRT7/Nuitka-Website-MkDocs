# Use Cases

This page discusses the various use cases of Nuitka, and it is recommended to be read entirely, since the cases build on each other. They will inform your decision on how to use Nuitka for your software.

## Program compilation with all modules embedded (acceleration)

If you want to compile a whole program recursively, and not only the single file that is the main program, do it like this:

```bash
python -m nuitka --follow-imports program.py
```

!!! note
    There are more fine-grained controls than `--follow-imports` available. Consider the output of `nuitka --help`. Including fewer modules into the compilation, but instead using normal Python for it, will make it faster to compile.

In case you have a source directory with dynamically loaded files, i.e. one which cannot be found by recursing after normal import statements via the `PYTHONPATH` (which would be the recommended way), you can always require that a given directory shall also be included in the executable:

```bash
python -m nuitka --follow-imports --include-plugin-directory=plugin_dir program.py
```

!!! note
    If you don't do any dynamic imports, simply setting your `PYTHONPATH` at compilation time is what you should do.

## Extension Module compilation

If you want to compile a single extension module, all you have to do is this:

```bash
python -m nuitka --module some_module.py
```

The resulting file `some_module.so` can then be used instead of `some_module.py`.

!!! important
    The filename of the produced extension module must not be changed as Python insists on a module name derived function as an entry point, in this case `PyInit_some_module` and renaming the file will not change that. Match the filename of the source code to what the binary name should be.

## Package compilation

If you need to compile a whole package and embed all modules, that is also feasible, use Nuitka like this:

```bash
python -m nuitka --module some_package --include-package=some_package
```

!!! note
    The inclusion of the package contents needs to be provided manually; otherwise, the package is mostly empty. You can be more specific if you like, and only include part of it, or exclude part of it, e.g. with `--nofollow-import-to='*.tests'` you would not include the unused test part of your code.

## Standalone Program Distribution

For distribution to other systems, there is the standalone mode, which produces a folder for which you can specify `--mode=standalone`.

```bash
python -m nuitka --mode=standalone program.py
```

Following all imports is default in this mode. You can selectively exclude modules by specifically saying `--nofollow-import-to`, but then an `ImportError` will be raised when import of it is attempted at program run time. This may cause different behavior, but it may also improve your compile time if done wisely.

For data files to be included, use the option `--include-data-files=<source>=<target>` where the source is a file system path, but the target has to be specified relative. For the standalone mode, you can also copy them manually, but this can do extra checks, and for the onefile mode, there is no manual copying possible.

To copy some or all file in a directory, use the option `--include-data-files=/etc/*.txt=etc/` where you get to specify shell patterns for the files, and a subdirectory where to put them, indicated by the trailing slash.

## Important Warnings

Nuitka does not consider data files code, do not include DLLs, or Python files as data files, and expect them to work, they will not, unless you really know what you are doing.
