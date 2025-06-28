# Nuitka PyQt5 Support

Information about PyQt5 compatibility and support in Nuitka.

!!! error "Problematic Support"
    Support for PyQt5 is relatively problematic. Better alternatives are strongly recommended.

## :material-alert: PyQt5 Support Issues

While the `pyqt5` plugin of Nuitka enables using it, there are known bugs with callbacks and threading. You can workaround them, but they can be very limiting.

### Known Problems

- **Callback Issues:** Problems with compiled methods as callbacks
- **Threading Bugs:** Various threading-related issues
- **Limited Workarounds:** Some issues can be worked around, but with significant limitations
- **Maintenance Status:** Limited ongoing support for PyQt5-specific issues

!!! warning "Production Risk"
    Using PyQt5 with Nuitka in production environments carries significant risk due to these known limitations.

## :material-trending-down: Why PyQt5 Has Poor Support

PyQt5 has several fundamental compatibility issues with Nuitka's compilation approach:

### Technical Challenges
- **Signal/Slot System:** Complications with compiled Python callbacks
- **Object Lifecycle:** Issues with Qt object management in compiled code
- **Threading Model:** Conflicts between PyQt5's threading and compiled code
- **Memory Management:** Reference counting issues in certain scenarios

### Development Priority
- **Legacy Framework:** PyQt5 is based on the older Qt5
- **Limited Resources:** Development focus has shifted to newer frameworks
- **Community Support:** Fewer contributors working on PyQt5 compatibility

## :material-lightbulb: Recommended Solutions

### Option 1: Nuitka Commercial with PySide2

The patched PySide2 source code and binary wheels for some platforms are available as part of the [Nuitka Commercial](../commercial.md) offering. The PySide2 and PyQt5 are most similar and might be the easiest way out.

**Benefits of PySide2 (Commercial):**
- ✅ Similar API to PyQt5
- ✅ Properly patched for Nuitka compatibility
- ✅ Professional support available
- ✅ Tested for production use

[Learn About Commercial PySide2](pyside2-support.md){ .md-button }

### Option 2: Migrate to Modern Frameworks

Otherwise your best bet is to migrate to **PyQt6** (which at this time does not support Qt threading with Nuitka) or **PySide6** (recommended) and use these, as there are no compatibility issues with these.

## :material-compare: Framework Comparison

| Framework | Nuitka Support | Qt Version | Recommendation |
|-----------|----------------|------------|----------------|
| **PySide6** | ✅ Excellent | Qt6 | **Highly Recommended** |
| **PyQt6** | ✅ Good | Qt6 | Good alternative |
| **PySide2** | ⚠️ Limited/Commercial | Qt5 | Only with Nuitka Commercial |
| **PyQt5** | ❌ Poor | Qt5 | **Not Recommended** |

### Why PySide6 is Recommended

- **Modern Framework:** Based on the latest Qt6
- **Excellent Compatibility:** No known major issues with Nuitka
- **Active Development:** Ongoing support and improvements
- **Performance:** Better performance than Qt5-based frameworks
- **Future-Proof:** Long-term support and development

## :material-migration: Migration Strategies

### From PyQt5 to PySide6

**API Similarities:**
- Very similar API structure
- Most code can be migrated with import changes
- Similar signal/slot system
- Compatible widget hierarchy

**Key Migration Steps:**
1. Change imports from `PyQt5` to `PySide6`
2. Update signal/slot connections (minor syntax differences)
3. Handle Qt6-specific API changes
4. Test thoroughly with Nuitka compilation

### From PyQt5 to PyQt6

**Considerations:**
- Same vendor (Riverbank Computing)
- Familiar licensing model
- Similar API patterns
- **Note:** Some threading limitations with Nuitka

[Qt6 Migration Resources](https://doc.qt.io/qt-6/portingguide.html){ .md-button target="_blank" rel="noopener" }

## :material-help: Getting Support

### For Current PyQt5 Users

If you must continue using PyQt5:

1. **Understand the Risks:** Accept that there will be limitations
2. **Test Thoroughly:** Extensive testing is crucial
3. **Avoid Complex Threading:** Minimize threading usage
4. **Plan Migration:** Start planning migration to supported frameworks

### Community Resources

- **[GitHub Issues](https://github.com/Nuitka/Nuitka/issues)** - Report specific PyQt5 problems
- **[Discord Community](https://discord.gg/nZ9hr9tUck)** - Get help from other users
- **[Migration Guides](https://doc.qt.io/qtforpython/)** - Official Qt migration documentation

## :material-code-json: Practical Recommendations

### Short-term Solutions

If you're currently using PyQt5 and need immediate solutions:

```python
# Basic workarounds for some issues
import sys
from PyQt5.QtWidgets import QApplication

# Ensure proper signal handling
app = QApplication(sys.argv)
app.setAttribute(Qt.AA_X11InitThreads, True)  # For threading issues
```

### Long-term Strategy

**Phase 1:** Assessment
- Audit your current PyQt5 usage
- Identify problematic areas (threading, complex callbacks)
- Plan migration timeline

**Phase 2:** Migration
- Start with PySide6 migration
- Update imports and API calls
- Test with Nuitka compilation

**Phase 3:** Optimization
- Leverage new Qt6 features
- Optimize for Nuitka compilation
- Implement proper error handling

## :material-rocket: Next Steps

Ready to move away from PyQt5? Here are your best options:

1. **[Migrate to PySide6](https://doc.qt.io/qtforpython/porting_from2.html)** - Best long-term solution
2. **[Try Nuitka Commercial with PySide2](../commercial.md)** - Easiest migration path
3. **[Get Community Help](../support.md)** - Support for migration process

[Choose Your Migration Path](../support.md){ .md-button .md-button--primary }

!!! success "Future-Proof Choice"
    Migrating to PySide6 not only solves current compatibility issues but also ensures your application is built on a modern, actively maintained framework with excellent Nuitka support.
