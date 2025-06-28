# Free Download

!!! info "Thank you for downloading Nuitka"
    Please consider becoming a Nuitka commercial subscriber.

<div class="responsive-google-slides">
    <iframe src="https://docs.google.com/presentation/d/e/2PACX-1vSQ8gKXjTPukmeULWnjqSWWOKzopxEQ-LqfPYbvHE4wEPuYTnj3JmYFc8fm-EriAYgXzEbI-kWwaaQN/embed?rm=minimal&start=true&loop=true&delayms=3000" frameborder="0" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>
</div>

## Nuitka Standard

The standard edition bundles your code, dependencies and data into a single executable if you want. It also does acceleration, just running faster in the same environment, and can produce extension modules as well. It is freely distributed under the Apache license.

[Get Nuitka Standard](/download/)

## Nuitka Commercial

The commercial edition additionally protects your code, data and outputs, so that users of the executable cannot access these. This a private repository of plugins that you pay to get access to. Additionally, you can purchase priority support.

[Learn more about Nuitka commercial](/commercial/)

The current release is Nuitka {{ nuitka_version }}. Stable releases are supported with hot fixes, indicated by the last of the 4 digits.

!!! note "Release Stability"
    Stable releases are supposed to work for you. Develop releases are snapshots of the current `develop` branch in git, usually also relatively stable, but also rarely break.

!!! warning "Package Build Timing"
    During releases package builds can lag behind for a couple of days.

## License

**Nuitka** is licensed under the [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0); you may not use it except in compliance with the License. Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an **"as is" basis, without warranties or conditions of any kind**, either express or implied. See the License for the specific language governing permissions and limitations under the License.

## Installation Methods

=== "PyPI"

    There is [Nuitka on PyPI](http://pypi.python.org/pypi/Nuitka/) as well. So you can install with `pip` as follows.

    !!! tip "Easy Installation"
        The stable version from PyPI can be installed via pip, and has no dependencies on any package, and is a source package, so you will have an easy time, even on e.g. Windows to use it.

    ```bash
    # Stable version
    python -m pip install -U nuitka

    # Develop version
    python -m pip install -U "https://github.com/Nuitka/Nuitka/archive/develop.zip"
    ```

    !!! note "Python Binary"
        Do this with the python binary, you want to be compiled against.

=== "Sources"

    The source archives can be used directly after unpacking, simply start with `python bin/nuitka --help` and read `README.pdf` or `README.rst` to get started. Take especially care to read the User Manual, such that you don't go on a wrong track.

    [Download source archives](https://github.com/Nuitka/Nuitka/releases)

=== "Git"

    **Stable:**
    ```bash
    git clone --branch main https://github.com/Nuitka/Nuitka
    ```

    **Develop:**
    ```bash
    git clone --branch develop https://github.com/Nuitka/Nuitka
    ```

    Visit [https://github.com/Nuitka/Nuitka](https://github.com/Nuitka/Nuitka) for the Nuitka repository on Github.

## Package Repositories

=== "Windows"

    The MSI installers are discontinued as Python has deprecated their support for them, as well as Windows 10 making it harder to users to install them. Using the PyPI installation is recommended on Windows.

=== "Debian/Ubuntu/Mint"

    === "Stable"

        ```bash
        CODENAME=`egrep 'UBUNTU_CODENAME|VERSION_CODENAME' /etc/os-release | sort | head -1 | cut -d= -f2`
        if [ -z "$CODENAME" ]
        then
           CODENAME=`lsb_release -c -s`
        fi
        wget -O - https://nuitka.net/deb/archive.key.gpg | sudo apt-key add -
        sudo apt-get install ca-certificates
        sudo echo >/etc/apt/sources.list.d/nuitka.list "deb https://nuitka.net/deb/stable/$CODENAME $CODENAME main"
        sudo apt-get update
        sudo apt-get install nuitka
        ```

    === "Develop"

        ```bash
        CODENAME=`egrep 'UBUNTU_CODENAME|VERSION_CODENAME' /etc/os-release | cut -d= -f2`
        if [ -z "$CODENAME" ]
        then
           CODENAME=`lsb_release -c -s`
        fi
        wget -O - https://nuitka.net/deb/archive.key.gpg | sudo apt-key add -
        sudo apt-get install ca-certificates
        sudo echo >/etc/apt/sources.list.d/nuitka.list "deb https://nuitka.net/deb/develop/$CODENAME $CODENAME main"
        sudo apt-get update
        sudo apt-get install nuitka
        ```

        !!! info "Standard Repository"
            Because Nuitka is part of Debian Stable/Testing/Unstable, a stable version is already in the standard repository. This is the only way to access the develop version of Nuitka though.

=== "RHEL"

    ```bash
    # Detect the RHEL version
    eval `grep VERSION_ID= /etc/os-release`

    yum-config-manager --add-repo http://download.opensuse.org/repositories/home:/kayhayen/RedHat_RHEL-${VERSION_ID}/home:kayhayen.repo

    # Install either of these, but not both
    yum install nuitka
    yum install nuitka-unstable
    ```

=== "CentOS"

    ```bash
    # CentOS 6:
    yum-config-manager --add-repo http://download.opensuse.org/repositories/home:/kayhayen/CentOS_CentOS-6/home:kayhayen.repo
    # CentOS 7
    yum-config-manager --add-repo http://download.opensuse.org/repositories/home:/kayhayen/CentOS_7/home:kayhayen.repo
    # CentOS 8
    yum-config-manager --add-repo http://download.opensuse.org/repositories/home:/kayhayen/CentOS_8/home:kayhayen.repo

    # Install either of these, but not both
    yum install nuitka
    yum install nuitka-unstable
    ```

=== "Fedora"

    ```bash
    # Detect the Fedora version
    eval `grep VERSION_ID= /etc/os-release`

    # Use yum on older versions
    dnf config-manager --add-repo https://download.opensuse.org/repositories/home:/kayhayen/Fedora_${VERSION_ID}/home:kayhayen.repo

    # Install either of these, but not both
    dnf install nuitka
    dnf install nuitka-unstable
    ```

=== "openSUSE"

    ```bash
    # Detect the OpenSUSE leap version
    eval `grep VERSION_ID= /etc/os-release`

    # Add Nuitka repo
    zypper ar -f https://download.opensuse.org/repositories/home:/kayhayen/Open_${VERSION_ID}/home:kayhayen.repo

    # Install either of these, but not both
    zypper install nuitka
    zypper install nuitka-unstable
    ```

=== "Arch Linux"

    **Stable:** Execute `pacman -S nuitka`

    **Develop:** [Nuitka from git develop](https://aur.archlinux.org/packages/nuitka-git/)

=== "Gentoo"

    Execute `emerge -a dev-python/nuitka`

=== "macOS"

    No installer is available for macOS. Use the source packages, clone from git, or use PyPI.
