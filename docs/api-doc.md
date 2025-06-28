# API Documentation

This document provides API documentation for Nuitka's internal interfaces and plugin system.

## Plugin API

Nuitka provides a plugin system that allows extending compilation behavior.

### Base Plugin Class

```python
class NuitkaPlugin:
    """Base class for Nuitka plugins."""
    
    def onModuleDiscovered(self, module):
        """Called when a module is discovered during compilation."""
        pass
    
    def considerExtraDlls(self, dist_dir, module):
        """Consider additional DLLs for a module."""
        return []
```

### Creating Plugins

To create a plugin:

1. Inherit from `NuitkaPlugin`
2. Override relevant methods
3. Register the plugin

### Examples

Example plugins can be found in the Nuitka source code under the `nuitka/plugins/` directory.

## Internal APIs

!!! warning
    Internal APIs are subject to change without notice. Use at your own risk.

### Code Generation

The code generation system provides interfaces for:

- AST transformation
- Code emission
- Optimization passes

### Type System

Nuitka's type system includes:

- Type shapes
- Value ranges
- Constraint propagation
