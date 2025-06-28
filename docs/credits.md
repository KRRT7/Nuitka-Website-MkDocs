# Credits

Why is Nuitka named like this, and who do we thank for it.

!!! heart "Acknowledgments"
    Nuitka is the result of countless contributions from individuals, projects, and the amazing Python community.

## :material-heart: Nuitka Namesake

The most credits are deserved by my ever loving and forgiving wife, who bears with me for spending literally all my spare and other time thinking of Nuitka.

See an image of her on Twitter and make her happy with donations and [Nuitka Commercial subscriptions](commercial.md)!

<blockquote class="twitter-tweet">
<p lang="en" dir="ltr">Nuitka is short for Annuitka, which is the nickname of my wife Anna who is Russian... here a recent shot with my son David.<br>I one day made her the compiler as a gift. Much better name than &quot;Py2C&quot;, right? <a href="https://t.co/9A3nod8CZ7">pic.twitter.com/9A3nod8CZ7</a></p>
&mdash; Kay Hayen (@KayHayen) <a href="https://twitter.com/KayHayen/status/1028940741047930880?ref_src=twsrc%5Etfw">August 13, 2018</a>
</blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## :material-account-group: Contributors to Nuitka

Thanks go to these individuals for their much-valued contributions to Nuitka. The order is sorted by time.

### Early Contributors

**Li Xuan Ji**
: Contributed patches for general portability issues and enhancements to the environment variable settings.

**Nicolas Dumazet**
: Found and fixed reference counting issues, `import` packages work, improved some of the English and generally made good code contributions all over the place, solved code generation TODOs, did tree building cleanups, core stuff.

**Khalid Abu Bakr**
: Submitted patches for his work to support MinGW and Windows, debugged the issues, and helped get cross compile with MinGW from Linux to Windows. This was quite difficult stuff.

**Liu Zhenhai**
: Submitted patches for Windows support, making the inline Scons copy actually work on Windows as well. Also reported import related bugs, and generally helped make the Windows port more usable through testing and information.

### Platform Support

**Christopher Tott**
: Submitted patches for Windows, and general as well as structural cleanups.

**Pete Hunt**
: Submitted patches for macOS X support.

**Johan Holmberg**
: Submitted patch for Python3 support on macOS X.

**David Cortesi**
: Submitted patches and test cases to make macOS port more usable, specifically for the Python3 standalone support of Qt.

### Core Features

**"ownssh"**
: Submitted patches for built-ins module guarding, and made massive efforts to make high-quality bug reports. Also the initial "standalone" mode implementation was created by him.

**Umbra**
: Submitted patches to make the Windows port more usable, adding user provided application icons, as well as MSVC support for large constants and console applications.

**Andrew Leech**
: Submitted GitHub pull request to allow using "-m nuitka" to call the compiler. Also pull request to improve `bist_nuitka` and to do the registration.

**Paweł K**
: Submitted GitHub pull request to remove glibc from standalone distribution, saving size and improving robustness considering the various distributions.

### Modern Contributors

**Orsiris de Jong**
: Submitted GitHub pull request to implement the dependency walking with `pefile` under Windows. Also provided the implementation of Dejong Stacks.

**Jorj X. McKie**
: Submitted GitHub pull requests with NumPy plugin to retain its accelerating libraries, and Tkinter to include the TCL distribution on Windows.

**Ben Fässler (alias "Fire Cube")**
: Lots of work on Nuitka Package configuration YAML configuration and mass migration of older Python plugin code to it, introducing the YAML schema for Nuitka Package configuration. Also added a well needed formatter that retains comments. Introduced the options-nanny plugin, and last but not least, helped with the website design, in particular polishing the CSS for the subscribe buttons to look very good.

### Creative Contributions

**Juan Carlos Paco**
: Submitted cleanup patches, created a Nuitka GUI and a Ninja IDE plugin for Nuitka. Both are no longer actively maintained though.

**"Dr. Equivalent"**
: Submitted the Nuitka Logo.

## :material-tools: Projects Used by Nuitka

Nuitka builds upon the shoulders of giants. Here are the key projects that make Nuitka possible:

### Core Dependencies

=== "CPython"

    **[The CPython Project](https://www.python.org/)**
    
    Thanks for giving us CPython, which is the base of Nuitka. We are nothing without it.

=== "GCC"

    **[The GCC Project](https://gcc.gnu.org/)**
    
    Thanks for not only the best compiler suite but also thanks for making it easy supporting to get Nuitka off the ground. Your compiler was the first usable for Nuitka and with very little effort.

=== "SCons"

    **[The SCons Project](https://www.scons.org/)**
    
    Thanks for tackling the difficult points and providing a Python environment to make the build results. This is such a perfect fit to Nuitka and a dependency that will likely remain.

### Development Tools

=== "Valgrind"

    **[The Valgrind Project](https://valgrind.org/)**
    
    Luckily we can use Valgrind to determine if something is an actual improvement without the noise. And it's also helpful to determine what's actually happening when comparing.

=== "Buildbot"

    **[The Buildbot Project](https://buildbot.net/)**
    
    Thanks for creating an easy to deploy and use continuous integration framework that also runs on Windows and is written and configured in Python code. This allows running the Nuitka tests long before release time.

=== "Code Quality"

    **[isort](https://timothycrosley.github.io/isort/)** - Thanks for making nice import ordering so easy.
    
    **[black](https://github.com/ambv/black)** - Thanks for making a fast and reliable way for automatically formatting the Nuitka source code.

### Infrastructure

=== "NeuroDebian"

    **[The NeuroDebian Project](https://neuro.debian.net/)**
    
    Thanks for hosting the build infrastructure that the Debian sponsor Yaroslav Halchenko uses to provide packages for all Ubuntu versions.

=== "openSUSE"

    **[The openSUSE Buildservice](https://openbuildservice.org/)**
    
    Thanks for hosting this excellent service that allows us to provide RPMs for a large variety of platforms and make them available immediately nearly at release time.

=== "MinGW64"

    **[The MinGW64 Project](https://mingw-w64.org/)**
    
    Thanks for porting the GCC to Windows. This allowed portability of Nuitka with relatively little effort.

## :material-gift: How to Contribute

Want to see your name here? There are many ways to contribute to Nuitka:

- **Code Contributions** - Submit pull requests for bug fixes and features
- **Documentation** - Help improve guides and documentation
- **Testing** - Report bugs and test new features
- **Package Configuration** - Add support for new Python packages
- **Financial Support** - Consider [Nuitka Commercial](commercial.md) or [donations](contribute.md)

[Learn How to Contribute](contribute.md){ .md-button .md-button--primary }

---

!!! quote "Thank You"
    The Nuitka project is made possible by the dedication and contributions of everyone listed here, as well as countless others who have helped through testing, feedback, and support. Thank you for making Nuitka better!
