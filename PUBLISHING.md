## Publishing

To publish a new version of the package to PyPI, follow these steps:

1. Update the version number in [pyproject.toml](pyproject.toml).
2. Build the package:
   ```bash
   python -m build

   ```
3. Make sure you have twine installed:
   ```bash
   pip install --upgrade build twine
   ```
4. Publish to PyPI:
   ```bash
   twine upload dist/*
   ```