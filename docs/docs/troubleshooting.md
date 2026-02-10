# DSPy 3.0 Dependency Troubleshooting Guide

This guide helps you resolve common dependency issues when upgrading to or installing DSPy 3.0.

## Quick Diagnosis

Run this diagnostic script to identify missing dependencies:

```python
def diagnose_dspy_installation():
    """Diagnose DSPy 3.0 installation and identify missing dependencies."""
    print("DSPy 3.0 Installation Diagnosis")
    print("=" * 40)

    # Test core DSPy
    try:
        import dspy
        print("✅ Core DSPy: OK")
        print(f"   Version: {dspy.__version__ if hasattr(dspy, '__version__') else 'Unknown'}")
    except ImportError as e:
        print(f"❌ Core DSPy: FAILED - {e}")
        return

    # Test datasets functionality
    try:
        from dspy.datasets import DataLoader, HotPotQA
        print("✅ Datasets: OK")
    except ImportError as e:
        print(f"❌ Datasets: FAILED - {e}")
        print("   Fix: pip install dspy[datasets]")

    # Test pandas functionality
    try:
        import pandas as pd
        print("✅ Pandas: OK")
    except ImportError as e:
        print(f"❌ Pandas: FAILED - {e}")
        print("   Fix: pip install pandas or pip install dspy[pandas]")

    # Test new 3.0 features
    try:
        audio = dspy.Audio.from_url("https://example.com/test.mp3")
        print("✅ Audio support: OK")
    except AttributeError:
        print("❌ Audio support: Not available (may need DSPy 3.0)")
    except Exception as e:
        print(f"⚠️  Audio support: Available but test failed - {e}")

    try:
        from dspy import XMLAdapter
        print("✅ XMLAdapter: OK")
    except ImportError:
        print("❌ XMLAdapter: Not available (may need DSPy 3.0)")

    try:
        from dspy import CodeAct
        print("✅ CodeAct: OK")
    except ImportError:
        print("❌ CodeAct: Not available (may need DSPy 3.0)")

# Run diagnosis
diagnose_dspy_installation()
```

## Common Error Scenarios

### 1. Dataset Import Errors

**Error:**

```
ImportError: No module named 'datasets'
ModuleNotFoundError: No module named 'dspy.datasets'
```

**Cause:** DSPy 3.0 moved datasets functionality to optional dependencies.

**Solutions:**

**Option A: Install with extras (Recommended)**

```bash
pip install dspy[datasets]
```

**Option B: Install dependencies manually**

```bash
pip install datasets pandas
```

**Option C: Install both datasets and pandas extras**

```bash
pip install dspy[datasets,pandas]
```

### 2. Pandas Import Errors

**Error:**

```
ImportError: No module named 'pandas'
AttributeError: module 'dspy' has no attribute 'DataFrame'
```

**Cause:** Pandas is no longer a core dependency in DSPy 3.0.

**Solutions:**

**Install pandas separately:**

```bash
pip install pandas
```

**Or use the pandas extra:**

```bash
pip install dspy[pandas]
```

### 3. Conflicting Dependencies

**Error:**

```
ERROR: pip's dependency resolver does not currently have a mechanism to handle version conflicts
```

**Cause:** Version conflicts between existing packages and DSPy 3.0 requirements.

**Solutions:**

**Option A: Clean install (Recommended)**

```bash
pip uninstall dspy
pip install --no-cache-dir dspy[datasets,pandas]
```

**Option B: Use virtual environment**

```bash
python -m venv dspy3_env
source dspy3_env/bin/activate  # On Windows: dspy3_env\Scripts\activate
pip install dspy[datasets,pandas]
```

**Option C: Force reinstall**

```bash
pip install --force-reinstall --no-deps dspy
pip install datasets pandas
```

### 4. Legacy Code Breaking

**Error:**

```
AttributeError: 'DataLoader' object has no attribute 'from_pandas'
ImportError: cannot import name 'HotPotQA' from 'dspy'
```

**Cause:** Import paths changed in DSPy 3.0.

**Migration Steps:**

**Before (DSPy 2.x):**

```python
from dspy import HotPotQA
from dspy import DataLoader
```

**After (DSPy 3.0):**

```python
# Install first: pip install dspy[datasets]
from dspy.datasets import HotPotQA, DataLoader
```

### 5. Audio Feature Errors

**Error:**

```
AttributeError: module 'dspy' has no attribute 'Audio'
```

**Cause:** Using an older version of DSPy or incomplete installation.

**Solutions:**

**Verify DSPy version:**

```python
import dspy
print(dspy.__version__)  # Should be 3.0.0 or higher
```

**Upgrade to DSPy 3.0:**

```bash
pip install --upgrade dspy
```

**If still failing, try clean install:**

```bash
pip uninstall dspy
pip install dspy>=3.0.0
```

### 6. XML Adapter Import Errors

**Error:**

```
ImportError: cannot import name 'XMLAdapter' from 'dspy'
```

**Cause:** Feature not available in your DSPy version.

**Solutions:**

**Check installation:**

```python
try:
    from dspy import XMLAdapter
    print("XMLAdapter available")
except ImportError:
    print("XMLAdapter not available - upgrade to DSPy 3.0")
```

**Upgrade if needed:**

```bash
pip install --upgrade dspy>=3.0.0
```

## Environment-Specific Issues

### Google Colab

```bash
# In Colab, use exclamation mark
!pip install dspy[datasets,pandas]

# Restart runtime after installation
# Runtime -> Restart Runtime
```

### Jupyter Notebooks

```bash
# Install in notebook cell
%pip install dspy[datasets,pandas]

# Restart kernel after installation
# Kernel -> Restart Kernel
```

### Docker Environments

**Dockerfile example:**

```dockerfile
FROM python:3.9

# Install DSPy with all extras
RUN pip install dspy[datasets,pandas]

# Or install specific versions
RUN pip install dspy>=3.0.0 datasets>=2.0.0 pandas>=1.3.0
```

### Conda Environments

```bash
# Create new environment
conda create -n dspy3 python=3.9
conda activate dspy3

# Install via pip (recommended)
pip install dspy[datasets,pandas]

# Or install dependencies via conda first
conda install pandas
pip install dspy[datasets]
```

## Advanced Troubleshooting

### Check Package Versions

```python
def check_versions():
    """Check versions of DSPy and related packages."""
    packages = ['dspy', 'datasets', 'pandas', 'numpy', 'transformers']

    for pkg in packages:
        try:
            module = __import__(pkg)
            version = getattr(module, '__version__', 'Unknown')
            print(f"{pkg}: {version}")
        except ImportError:
            print(f"{pkg}: Not installed")

check_versions()
```

### Verify Installation Integrity

```python
def verify_dspy_features():
    """Verify that DSPy 3.0 features are working."""
    import dspy

    # Test basic functionality
    try:
        sig = dspy.Signature("input -> output")
        print("✅ Basic DSPy functionality works")
    except Exception as e:
        print(f"❌ Basic functionality failed: {e}")

    # Test new features
    features = {
        'Audio': lambda: hasattr(dspy, 'Audio'),
        'XMLAdapter': lambda: hasattr(dspy, 'XMLAdapter'),
        'CodeAct': lambda: hasattr(dspy, 'CodeAct'),
    }

    for feature, test in features.items():
        if test():
            print(f"✅ {feature} available")
        else:
            print(f"❌ {feature} not available")

verify_dspy_features()
```

### Clean Reinstall Script

```bash
#!/bin/bash
# clean_install_dspy3.sh

echo "Cleaning existing DSPy installation..."
pip uninstall -y dspy datasets pandas

echo "Clearing pip cache..."
pip cache purge

echo "Installing DSPy 3.0 with all extras..."
pip install --no-cache-dir dspy[datasets,pandas]

echo "Verifying installation..."
python -c "
import dspy
from dspy.datasets import DataLoader
print('✅ DSPy 3.0 installation successful!')
print(f'Version: {getattr(dspy, \"__version__\", \"Unknown\")}')
"
```

## Prevention Tips

### 1. Use Virtual Environments

Always use virtual environments to avoid dependency conflicts:

```bash
python -m venv dspy3_project
source dspy3_project/bin/activate
pip install dspy[datasets,pandas]
```

### 2. Pin Dependencies

In your `requirements.txt`:

```txt
dspy>=3.0.0
datasets>=2.0.0
pandas>=1.3.0
```

### 3. Test After Installation

Always run a quick test after installation:

```python
# test_installation.py
import dspy
from dspy.datasets import DataLoader

print("✅ DSPy 3.0 ready to use!")
```

### 4. Keep Documentation Updated

Bookmark the official DSPy 3.0 migration guide and refer to it when upgrading existing projects.

## Getting Additional Help

If these troubleshooting steps don't resolve your issue:

1. **Check DSPy GitHub Issues**: Search for similar problems
2. **Create Minimal Reproduction**: Isolate the problem in a simple script
3. **Include Environment Details**: Python version, OS, package versions
4. **Check Official Documentation**: Visit the DSPy documentation for updates

## Quick Reference Commands

```bash
# Full clean install
pip uninstall dspy && pip install dspy[datasets,pandas]

# Check what's installed
pip list | grep -E "(dspy|datasets|pandas)"

# Verify DSPy features
python -c "import dspy; from dspy.datasets import DataLoader; print('OK')"

# Update everything
pip install --upgrade dspy[datasets,pandas]
```
