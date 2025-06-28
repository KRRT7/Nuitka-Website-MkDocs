# Nuitka Developer Manual

The purpose of this Developer Manual is to present the current design of Nuitka, the project rules, and the motivations for choices made. It is intended to be a guide to the source code, and to give explanations that don't fit into the source code in comments form.

It should be used as a reference for the process of planning and documenting decisions we made. Therefore we are e.g. presenting here the type inference plans before implementing them. And we update them as we proceed.

It grows out of discussions and presentations made at conferences as well as private conversations or issue tracker.

## Milestones

1. **Feature parity with CPython**, understand all the language construct and behave absolutely compatible.

   Feature parity has been reached for CPython 2.6 and 2.7. We do not target any older CPython release. For CPython 3.4 up to 3.13 it also has been reached (not all 3.12+ features are currently fully working though). We do not target the older and practically unused CPython 3.0 to 3.3 releases.

   This milestone was reached. Dropping support for Python 2.6 and 3.3 is an option, should this prove to be any benefit. Currently it is not, as it extends the test coverage only.

2. **Create the most efficient native code** from this. This means to be fast with the basic Python object handling.

   This milestone was reached, although of course, micro optimizations to this are happening all the time.

3. **Constant propagation**, determine as many values and useful constraints as possible at compile time and create more efficient code.

   This milestone is considered almost reached. We continue to discover new things, but the infrastructure is there, and these are easy to add.

4. **Type inference**, detect and special case the handling of strings, integers, lists in the program.

   This milestone is considered in progress.

5. **Add interfacing to C code**, so Nuitka can turn a `ctypes` binding into an efficient binding as written with C.

   This milestone is planned only.

6. **Add hints module** with a useful Python implementation that the compiler can use to learn about types from the programmer.

   This milestone is planned only.

## Version Numbers

For Nuitka we use semantic versioning, initially with a leading zero still, once we pass release `1.9`, the scheme will indicate the `10` through using `2.0`.

## Current State

Nuitka top level works like this:

- `nuitka.tree.Building` outputs node tree
- `nuitka.optimization` enhances it as best as it can
- `nuitka.finalization` prepares the tree for code generation
- `nuitka.code_generation.CodeGeneration` orchestrates the creation of code snippets
- `nuitka.code_generation.*Codes` knows how specific code kinds are created
- `nuitka.MainControl` keeps it all together

This design is intended to last.

Regarding types, the state is:

- Types are always `PyObject *`, and only a few C types, e.g. `nuitka_bool` and `nuitka_void` and more are coming. Even for objects, often it's know that things are e.g. really a `PyTupleObject **`, but no C type is available for that yet.

- There are a some specific use of types beyond "compile time constant", that are encoded in type and value shapes, which can be used to predict some operations, conditions, etc. if they raise, and result types they give.

- In code generation, the supported C types are used, and sometimes we have specialized code generation, e.g. a binary operation that takes an `int` and a `float` and produces a `float` value. There will be fallbacks to less specific types.

The expansion with more C types is currently in progress, and there will also be alternative C types, where e.g. `PyObject *` and `C long` are in an enum that indicates which value is valid, and where special code will be available that can avoid creating the `PyObject **` unless the later overflows.

## Setting up the Development Environment for Nuitka

Currently there are very different kinds of files that we need support for. This is best addressed with an IDE. We cover here how to setup the most common one.

### Visual Studio Code

Download Visual Studio Code from here: https://code.visualstudio.com/download

At this time, this is the recommended IDE for Linux and Windows. This is going to cover the plugins to install. Configuration is part of the `.vscode` in your Nuitka checkout. If you are not familiar with Eclipse, this is Free Software IDE,designed to be universally extended, and it truly is. There are plugins available for nearly everything.

The extensions to be installed are part of the Visual Code recommendations in `.vscode/extensions.json` and you will be prompted about that and ought to install these.

### Eclipse / PyCharm

Don't use these anymore, we consider Visual Studio Code to be far superior for delivering a nice out of the box environment.

## Commit and Code Hygiene

In Nuitka we have tools to auto format code, you can execute them manually, but it's probably best to execute them at commit time, to make sure when we share code, it's already well format, and to avoid noise doing cleanups.

The kinds of changes also often cause unnecessary merge conflicts, while the auto format is designed to format code also in a way that it avoids merge conflicts in the normal case, e.g. by doing imports one item per line.

In order to set up hooks, you need to execute these commands:

```bash
# Where python is the one you use with Nuitka, this then gets all
# development requirements, can be full PATH.
python -m pip install -r requirements-devel.txt
python ./misc/install-git-hooks.py
```

These commands will make sure that the `autoformat-nuitka-source` is run on every staged file content at the time you do the commit. For C files, it may complain unavailability of `clang-format`, follow it's advice. You may call the above tool at all times, without arguments to format call Nuitka source code.

Should you encounter problems with applying the changes to the checked out file, you can always execute it with `COMMIT_UNCHECKED=1` environment set.

## Coding Rules Python

These rules should generally be adhered when working on Nuitka code. It's not library code and it's optimized for readability, and avoids all performance optimization for itself.

### Tool to format

There is a tool `bin/autoformat-nuitka-source` which is to apply automatic formatting to code as much as possible. It uses `black` (internally) for consistent code formatting. The imports are sorted with `isort` for proper order.
