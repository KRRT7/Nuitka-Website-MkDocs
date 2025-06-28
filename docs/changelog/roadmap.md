# Nuitka Roadmap

This is the Nuitka roadmap, broken down by features.

## User Extensibility

- Data files, implicit imports, and DLL inclusion are specified in Yaml files now.

  A post series is currently going on and has been launched at post: [Nuitka Package Config Kickoff](https://nuitka.net/posts/nuitka-package-config-kickoff.html) and it will continue to improve the documentation that currently lives under [Nuitka Package Config](https://nuitka.net/doc/nuitka-package-config.html) on the web site only for rapid development independent of Nuitka releases.

  The long term plan is to also include in the Nuitka release as part of the documentation, much like User Manual and Developer Manual, that are being maintained inside Nuitka repo.

  The standard Yaml files (if modified) should be checked at runtime of Nuitka, for that we need to add some kind of checksum to it to detect modification and issue a warning, if `jsonschema` is not available for modification. Vendoring it seems unnecessarily much effort, and it's in `requirements-devel.txt` anyway.

  Currently the checksums are added in the commit hook, but they are not checked at runtime. We might want to limit checking to only used configuration entries.

## Onefile speed (standard)

- Use Windows NTFS and macOS HFS extended attributes to store caching status of a file inside of it. It might be possible to detect modification of the file in this way and spare us the checksum, which will then be used only in case of a fallback being necessary.

  Example code for Windows can be found here: https://github.com/microsoft/Windows-classic-samples/blob/main/Samples/Win7Samples/winui/shell/appplatform/PropertyEdit/PropertyEdit.cpp

- All files are compressed individually, we might then be able to cache the result of a specific file, such that files from the Python installation do not have to be redone over and over.

- Write payload files as memory mapped too, that too should be faster.

## Python 3.11

- Attribute lookups for types with a generic one need to update that code path, they will be much slower in 3.11 until we do that. That breaks the performance. We want to cleanup the code, potentially sharing improvements by generating code variants rather that duplicating stuff.

- Nested async compressions are not yet fully compatible, some newly allowed, strange forms, are failing `test_testcoroutines` and need to be supported eventually, although real code is very unlikely to encounter it.

## Python 3.12

- Use special code for 2 digits code in the long operation templates. Currently only single digit is optimized, but with Python 3.12, we can do better now.

- Add support for generic classes, these are not yet implemented which is not acceptable mid-term.

## Python 3.13

- The No-GIL variant of Python 3.13 is not currently working with Nuitka and needs more work. Right now it seems we cannot import `inspect` without crashing, we need to audit data structures for more issues like one we found with list data allocations.

## Nuitka-Python (standard)

This is currently under way and not yet described here. The current Nuitka release has support for using it. Most work is focused on the aim of getting it capable to statically compile, avoiding extension modules and DLL usages.

## Scalability (standard)

- **More compact code objects handling**

  Code objects and their creation is among the oldest code in Nuitka and lacks 2 features. First, their creation cannot be delayed, so they consume memory even if never used and module load time as well.

  We have since began to create constants from binary blobs. These too are also always created before use, but in some cases, we want to become able to delay this step.

  !!! note
      As of Nuitka 2.6, there is an experimental flag to enable these, and we need to switch over to using it.

- **Enhanced tracing of loop exit merges**

  Tracing of exception exits is not done for function exits and module exits at this time, meaning that the most merge intensive form of tracing is not applied. However, with a for loop, and a bunch of code on the inside, even if the actual exception exit doesn't matter much more than if it happens at all, or for only very few variables (iterated, iterated value, etc.) causes a full blown tracing to be done. Experiments have shown, that this for some modules causing a 40% increase of work to do, and providing the most complex merges to be done, which end up being used.

- **More scalable class creation**

  For class creation, we have a bunch of complexity. We cannot decide easily if a class dictionary (while being in the class scope) is a normal dict, or at least well behaving like it, or if it's some sort of magic thing that changes all your assignments to something else, and won't allow reading them back, etc. as all of that happens potentially.

## Performance (standard)

- **Dual types**

  After recent improvements, loop analysis became strong enough to trace the types of loop variables, when integer operations are used to increment them, but not if they come out of iterators. That should be added.

- **Function inlining**

  There is dead code in Nuitka capable of inlining functions, but it is not used. It should be used on the complex call helpers when arguments are constant, maybe even with hints towards loop unrolling, where there are loops e.g. over dictionaries.

- **Static metaclass and class dictionaries for Python3**

  Changes in 1.5 allow this for the case of no base class being specified. But if even only `object` is given a base class, then it changes to not being compile time resolved.

## macOS enhancements

- While `arm64` (M1) only builds and `x86_64` (Intel) only builds work, the value `universal` which of course implies twice the size, and as such has other disadvantages, is not yet supported.

## Container Builds (public + commercial)

Providing containers with old Linux, and optimally compiled CPython with `podman` such that building with Nuitka on Fedora latest and Ubuntu latest can be done fully automatically and still run on very old Linux.

## Automatic Updates

The running application needs to check for updates, and update itself automatically, optionally after user prompt, on a restart, or after successful update.

This has been implemented for onefile mode only. Unfortunately that is not good for macOS which often require app mode, i.e. standalone mode effectively with more than a single file.

## Traceback Encryption (commercial)

- Right now tracebacks are entirely encrypted. But in a future update, you can decide which information is transferred, and what information is part of the encryption, and which part is not, e.g. hostname, client name, etc. could be output in plain text, while the variable names and values would not be, depending on your choice!

- Dejong Stacks: More robust parser that allows stdout and stderr in same file with mixed outputs.

## Features to be added for Nuitka 2.8

- [x] Activate more scalable code objects handling
- [ ] Enhanced tracing of loop exit merges
- [ ] More scalable class creation

## Features to be added for Nuitka 2.9

- [ ] Use performance potential for attribute access with Python 3.11 version.
- [ ] Document commercial file embedding publicly with examples.
- [ ] Document commercial Windows Service usage with examples.
- [ ] Document traceback encryption usage with examples.

## Features to be added for Nuitka 3.0

- [ ] Initial support for ctypes based direct calls of C code.
- [ ] Tuple unpacking for values that support indexing should be optimized.
- [ ] Add download updating for standalone as well, onefile for windows works.
