# Nuitka Package Configuration

## Introduction

For packaging, and compatibility, some Python packages need to have special considerations in Nuitka. Some will not work without certain data files, sometimes modules depend on other modules in a hidden way, and for standalone DLLs might have to be included, that are loaded dynamically and therefore also invisible.

Another area is compatibility hacks, and removing bloat from code or just making sure, you are not using an unsupported version or wrong options for a package.

To make it easier to deal with missing DLLs, implicit imports, data files, bloat etc. Nuitka has a system with Yaml files. These ship inside of it and are located under `plugins/standard` and are designed to be easily be extended.

The structure of the filename is always `*nuitka-package.config.yml`. The `standard` file includes all things that are not in the standard library (`stdlib`) of Python. In `stdlib2` and `stdlib3` there are entries for the standard library. In `stdlib2` there are only those for modules that are no longer available in Python3.

If you want to use your own configuration, you can do so by passing the filename of your Yaml file via the `--user-package-configuration-file=my.nuitka-package.config.yml` option.

If it could be interesting for the other parts of the user base of Nuitka, please do a PR that adds it to the general files. In this way, not every user has to repeat what you just did, and we can collectively maintain it.

## The YAML Configuration File

At the beginning of the file you will find the following lines, which you can ignore, they are basically only there to silence checkers about problems that are too hard to avoid.

```yaml
# yamllint disable rule:line-length
# yamllint disable rule:indentation
# yamllint disable rule:comments-indentation
# too many spelling things, spell-checker: disable
---
```

An entry in the file look like this:

```yaml
- module-name: 'pandas._libs'
  implicit-imports:
    - depends:
        - 'pandas._libs.tslibs.np_datetime'
        - 'pandas._libs.tslibs.nattype'
        - 'pandas._libs.tslibs.base'
```

The `module-name` value is the name of the affected module. We will show and explain to you everything the other things in detail later. But the key principle is that a declaration always references a module by name.

It is also important to know that you do not have to worry about formatting. We have programmed our own tool for this, which formats everything automatically. This is executed via `bin\autoformat-nuitka-source` and automatically when pushing with `git` if you install the git hook (see Developer Manual for that).

There is also a Yaml schema file to check your files against and that in Visual Code is automatically applied to the Yaml files and that then supports you with auto-completion in Visual Code. So actually doing the change in PR form can be easier than not.

## Configuration Features

### Data Files

```yaml
data-files:
  dest_path: '.' # default, relative to package directory, normally not needed
  dirs:
    - 'dir1'

  patterns:
    - 'file1'
    - '*.dat'

  empty_dirs:
    - 'empty_dir'

  empty_dir_structures:
    - 'empty_dir_structure'

  when: 'win32'
```

If a module needs data files, you can get Nuitka to copy them into the output with the following features.

#### Features

- `dest_path`: target directory
- `dirs`: all directories that should be copied
- `patterns`: all files that should be copied (filename can be a [glob pattern](https://docs.python.org/3/library/glob.html#glob.glob))
- `empty_dirs`: all empty directories that should be copied
- `empty_dir_structures`: all empty directory structures that should be copied
- `when`: conditional inclusion based on platform

#### Examples

**Example 1: Simple data folder**

The most simple form just adds a data folder. The data files are in a folder and lives inside the package directory.

```yaml
- module-name: 'customtkinter'
  data-files:
     dirs:
       - 'assets'
```

**Example 2: Complete folder with data files**

This example includes a complete folder with data files in a package.

```yaml
- module-name: 'tkinterweb'
  data-files:
    dirs:
      - 'tkhtml'
```

**Example 3: Empty folder creation**

This example will make sure an empty folder is created relative to a package.

```yaml
- module-name: 'Crypto.Util._raw_api'
  data-files:
    empty_dirs:
      - '.'
```

!!! note
    The reason this is necessary is that some packages expect to have their directory as derived from `__file__` to exist. But for compiled packages, unless there is extension packages or data files copied into them, these directories do not exist.

### DLLs

```yaml
dlls:
  - from_filenames:
      relative_path: 'dlls'
```

For Windows standalone distributions, DLL files might need to be included that are loaded at runtime.

### Implicit Imports

```yaml
implicit-imports:
  - depends:
      - 'module_name1'
      - 'module_name2'
```

Some modules import other modules in ways that Nuitka cannot detect statically. These hidden dependencies need to be specified.

### Anti-Bloat

```yaml
anti-bloat:
  - replacements_plain:
      'old_text': 'new_text'
    when: 'not deployment'
```

Anti-bloat configurations help reduce the size of compiled applications by removing or replacing unnecessary code.

## Best Practices

1. **Test your configurations** thoroughly before submitting
2. **Use minimal configurations** - only include what's necessary
3. **Document complex cases** with comments in the YAML
4. **Consider platform differences** using the `when` condition
5. **Submit improvements** back to the community via Pull Requests

## Getting Help

If you encounter issues with package configurations:

1. Check the existing configurations in `plugins/standard`
2. Search for similar packages in the configuration files
3. Test with minimal examples
4. Report issues with detailed reproduction steps
5. Consider contributing your solutions back to the project
