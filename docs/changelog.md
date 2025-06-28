# Release Information

This page provides comprehensive information about Nuitka releases, including current changes, roadmap, and version history.

!!! info "Stay Current"
    Keep track of the latest Nuitka developments, bug fixes, and new features across all release versions.

## :material-map: Overview

=== "Roadmap"

    **Future Planning**
    
    This is the roadmap of Nuitka, where we want to be in the next couple of releases.
    
    [View Roadmap](changelog/roadmap.md){ .md-button }

=== "Current Release"

    **Version 2.7** *(Latest)*
    
    Read about changes from the current release of Nuitka with all the latest features and improvements.
    
    [View Current Release](changelog/current.md){ .md-button .md-button--primary }

=== "Next Release"

    **Upcoming Changes**
    
    Read about changes for the upcoming release, which also includes hotfix information for the current release.
    
    [View Upcoming Changes](changelog/next.md){ .md-button }

## :material-timeline: Release Series

### :material-numeric-2-box: Version 2.x Series

The current major release series with the latest features and improvements.

[View 2.x Changelog](changelog/changelog-2x.md){ .md-button }

### :material-numeric-1-box: Version 1.x Series

Changes for the 1.x major release series - the previous stable series.

[View 1.x Changelog](changelog/changelog-1x.md){ .md-button }

### :material-numeric-0-box: Version 0.x Series

Historical changes for the 0.x major release series - the original development series.

[View 0.x Changelog](changelog/changelog-0x.md){ .md-button }

## :material-rocket-launch: Latest Release Highlights

### Nuitka Release 2.7

This release adds a ton of new features and corrections, including:

#### :material-bug-check: Major Bug Fixes

- **macOS:** Correctly recognize self-dependencies of DLLs with architecture suffix
- **Standalone:** Resolved `.pyi` file detection and parsing issues
- **Windows:** Fixed console mode attachment and MSVC compilation issues
- **Python 3.12+:** Improved type alias compatibility and extension module context handling

#### :material-feature-search: New Features

- Enhanced compatibility with newer Python versions
- Improved standalone distribution handling
- Better DLL dependency resolution
- Enhanced package configuration support

#### :material-speedometer: Performance Improvements

- Optimized compilation processes
- Better memory management
- Faster startup times for compiled applications

[View Full Release Notes](changelog/current.md){ .md-button }

## :material-history: Quick Navigation

| Version | Release Date | Key Features |
|---------|-------------|--------------|
| **2.7** | May 2025 | Enhanced compatibility, bug fixes |
| **2.6** | Previous | Stability improvements |
| **2.5** | Earlier | Performance enhancements |
| **2.0** | Major | Python 3.11+ support |

## :material-download: Download Current Release

Ready to try the latest version?

[Download Nuitka 2.7](download.md){ .md-button .md-button--primary }

## :material-github: Release Tracking

Stay informed about new releases:

- **[GitHub Releases](https://github.com/Nuitka/Nuitka/releases)** - Official release announcements
- **[Twitter Updates](https://x.com/kayhayen)** - Quick release notifications  
- **[Discord Community](https://discord.gg/nZ9hr9tUck)** - Community discussions about releases

## :material-file-search: What's in a Release?

Each Nuitka release typically includes:

### :material-bug: Bug Fixes
- Platform-specific fixes (Windows, macOS, Linux)
- Python version compatibility improvements
- Standalone distribution corrections
- Plugin and package compatibility fixes

### :material-plus-circle: New Features
- Enhanced compilation options
- Improved package support
- New optimization techniques
- Extended platform support

### :material-speedometer: Performance Improvements
- Faster compilation times
- Better runtime performance
- Memory usage optimizations
- Startup time improvements

### :material-tools: Developer Experience
- Better error messages
- Enhanced debugging support
- Improved documentation
- New configuration options

!!! tip "Release Schedule"
    Nuitka follows a regular release schedule with frequent updates. Major releases introduce new features, while minor releases focus on bug fixes and stability improvements.
