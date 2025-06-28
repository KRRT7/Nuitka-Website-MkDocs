# Nuitka PySide2 Support

Information about PySide2 compatibility and support in Nuitka.

!!! warning "Limited Baseline Support"
    Support with baseline PySide2 is only partial. Enhanced support is available through Nuitka Commercial.

## :material-alert-circle: Baseline PySide2 Limitations

While the `pyside2` plugin of Nuitka can workaround some issues PySide 5.15.2.1 has to support compiled methods as callbacks, these are far from complete. Some programs work, but many do have problems, especially in reacting to events.

### Known Issues

- **Callback Methods:** Compiled methods as callbacks have incomplete support
- **Event Handling:** Many programs have problems reacting to events
- **Threading Issues:** Various threading-related complications
- **Partial Functionality:** Some features work, but others may fail unexpectedly

!!! example "Compatibility Status"
    - ✅ Basic GUI applications may work
    - ❌ Complex event-driven applications often have issues  
    - ❌ Threading applications are problematic
    - ❌ Callback-heavy applications may fail

## :material-check-circle: Enhanced Support in Nuitka Commercial

For this, the PySide2 wheels provided by [Nuitka Commercial](../commercial.md) contain a backport of the PySide6 changes made to support compiled code perfectly. The support of PySide6 is much better, but unfortunately not perfect. However, all current known bugs with PySide6 do not affect PySide2 as accessible in Nuitka Commercial.

### What's Included

The patched PySide2 source code and binary wheels for some platforms are available as part of the [Nuitka Commercial](../commercial.md) offering.

**Benefits of Commercial PySide2:**
- **Perfect Compilation Support:** Backported PySide6 improvements
- **Event Handling:** Properly working event system
- **Callback Methods:** Full support for compiled methods as callbacks
- **Stability:** Tested and verified for production use

!!! success "Commercial Solution"
    The recommended solution for serious PySide2 development is to use the wheels as provided by Nuitka Commercial. There is no way these patches will be backported to the official PySide2 release.

## :material-lightbulb: Why No Official Backport?

For older PySide2 and just generally, the recommended solution is to use the wheels as provided by Nuitka Commercial. There is no way these patches will be backported and as a result, we have that backported patch that will not get released in any other way. This is very understandable, now that Qt6 is out.

The Qt project has moved focus to Qt6/PySide6, making backports to PySide2 unlikely.

## :material-arrow-right: Recommended Alternatives

If Nuitka Commercial is not an option, consider these alternatives:

### Migration to PySide6

Otherwise your best bet is to migrate to **PySide6** which has excellent support for Nuitka.

**PySide6 Benefits:**
- ✅ Excellent Nuitka compatibility
- ✅ Active development and support
- ✅ Modern Qt6 features
- ✅ Better performance
- ✅ Long-term support

[Learn more about PySide6 migration](https://doc.qt.io/qtforpython/porting_from2.html){ .md-button target="_blank" rel="noopener" }

### PyQt6 Option

Also **PyQt6** would work, but **PyQt5** unfortunately is very poorly supported.

**Framework Compatibility:**
- ✅ **PySide6** - Excellent support (Recommended)
- ✅ **PyQt6** - Good support 
- ⚠️ **PySide2** - Limited (Commercial patches available)
- ❌ **PyQt5** - Poor support

## :material-help: Getting Help

### For Nuitka Commercial Users

If you're using Nuitka Commercial with PySide2:
- Access to patched PySide2 wheels
- Priority support for PySide2 issues
- Direct access to Kay Hayen for complex problems

[Purchase Nuitka Commercial](../commercial/purchase.md){ .md-button .md-button--primary }

### For Community Users

If you're using the free version:
- Consider migrating to PySide6
- Use the `pyside2` plugin with awareness of limitations
- Report issues on [GitHub](https://github.com/Nuitka/Nuitka/issues)
- Join the [Discord community](https://discord.gg/nZ9hr9tUck) for help

[Get Community Support](../support.md){ .md-button }

## :material-migration: Migration Guide

Ready to move to PySide6? Here's what you need to know:

### Key Changes
- Import statements change from `PySide2` to `PySide6`
- Some API changes in Qt6
- Better performance and modern features
- Excellent Nuitka compatibility

### Resources
- [Official PySide6 Migration Guide](https://doc.qt.io/qtforpython/porting_from2.html)
- [Qt6 Porting Guide](https://doc.qt.io/qt-6/portingguide.html)
- [Nuitka Package Configuration](../package-configuration.md) for PySide6

!!! tip "Migration Support"
    The Nuitka community can help with migration questions. Join our Discord or check the documentation for PySide6-specific guidance.
