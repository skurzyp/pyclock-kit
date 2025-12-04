# 🚀 Publishing PyClockKit to PyPI

This guide covers two methods for publishing your package to PyPI: **automated releases via GitHub Actions** (recommended) and **manual publishing**.

---

## Table of Contents

- [Automated Publishing (Recommended)](#-method-1-automated-publishing-via-github-actions-recommended)
- [Manual Publishing](#-method-2-manual-publishing-with-twine)

---

## 🤖 Method 1: Automated Publishing via GitHub Actions (Recommended)

This method uses GitHub Actions to automatically publish to PyPI when you push a version tag. It uses **PyPI Trusted Publishing**, which is more secure and doesn't require managing API tokens.

### Prerequisites

1. **PyPI Account**: Create an account at [https://pypi.org/account/register/](https://pypi.org/account/register/)
2. **GitHub Repository**: Your code must be in a GitHub repository

### Step 1: Set Up PyPI Trusted Publishing

1. **Log in to PyPI** and go to your account settings
2. Navigate to **Publishing** → **Add a new pending publisher**
3. Fill in the following details:
   - **PyPI Project Name**: `pyclock-kit`
   - **Owner**: Your GitHub username (e.g., `yourusername`)
   - **Repository name**: Your repository name (e.g., `pyclock-kit`)
   - **Workflow name**: `release.yml`
   - **Environment name**: `pypi`
4. Click **Add**

> **Note**: The trusted publisher will remain "pending" until the first successful workflow run creates the project.

### Step 2: Prepare Your Release

1. **Update the version in `pyproject.toml`**:
   ```bash
   # Edit pyproject.toml and update the version field
   version = "0.1.0"  # Change to your desired version
   ```

2. **Commit your changes**:
   ```bash
   git add pyproject.toml
   git commit -m "Bump version to 0.1.0"
   git push
   ```

### Step 3: Create and Push a Tag

1. **Create a version tag** (must match `v*.*.*` pattern):
   ```bash
   git tag v0.1.0
   ```

2. **Push the tag to GitHub**:
   ```bash
   git push origin v0.1.0
   ```

### Step 4: Monitor the Release

1. Go to your GitHub repository → **Actions** tab
2. You should see the **"Publish to PyPI"** workflow running
3. Once complete, your package will be live at `https://pypi.org/project/pyclock-kit/`

### 🎉 Done!

Your package is now published! Users can install it with:
```bash
pip install pyclock-kit
```

---

## 🛠️ Method 2: Manual Publishing with Twine

Use this method if you prefer manual control or need to publish without GitHub Actions.

### Step 1: Prerequisites (Account & API Token)

1. **Create a PyPI Account**:
   - Go to [https://pypi.org/account/register/](https://pypi.org/account/register/)

2. **Generate an API Token**:
   - Log in to PyPI → **Account Settings** → **API Tokens**
   - Click **Add API token**
   - Give it a name (e.g., "Manual Upload")
   - **Scope**: Choose **Entire account (all projects)** for first upload, or select specific project
   - **CRITICAL**: Copy the token (starts with `pypi-`) and save it securely - it's shown only once!

### Step 2: Prepare and Build the Package

1. **Update version in `pyproject.toml`**:
   ```toml
   version = "0.1.0"  # Update this
   ```

2. **Install/upgrade build tools**:
   ```bash
   pip install --upgrade build twine
   ```

3. **Clean previous builds** (optional but recommended):
   ```bash
   rm -rf dist/
   ```

4. **Build the package**:
   ```bash
   python -m build
   ```
   
   This creates distribution files in the `dist/` directory.

### Step 3: Publish to PyPI

1. **Upload with Twine**:
   ```bash
   twine upload dist/*
   ```

2. **Authentication**:
   - **Username**: `__token__`
   - **Password**: Paste your API token (including the `pypi-` prefix)

   Or set up a `~/.pypirc` file:
   ```ini
   [pypi]
   username = __token__
   password = pypi-YOUR_API_TOKEN_HERE
   ```

### Step 4: Verify the Upload

1. Visit `https://pypi.org/project/pyclock-kit/`
2. Verify the version and metadata are correct

### 🎉 Done!

Your package is published! Test the installation:
```bash
pip install pyclock-kit
```

---

## 📝 Publishing Checklist

Before publishing (either method), ensure:

- [ ] Version number updated in `pyproject.toml`
- [ ] All tests pass: `pytest`
- [ ] Code is formatted: `black .`
- [ ] No linting errors: `flake8`
- [ ] Type checking passes: `mypy pyclock`
- [ ] README.md is up to date
- [ ] CHANGELOG updated (if applicable)
- [ ] Changes committed and pushed to GitHub

---

## 🆘 Troubleshooting

### "File already exists" error
You cannot overwrite an existing version on PyPI. Increment your version number and try again.

### "Invalid or non-existent authentication information"
- For manual publishing: Check your API token is correct and includes the `pypi-` prefix
- For automated publishing: Verify your PyPI Trusted Publishing configuration matches your GitHub repository details

### Workflow doesn't trigger
- Ensure your tag matches the pattern `v*.*.*` (e.g., `v0.1.0`, not `0.1.0`)
- Check the workflow file is at `.github/workflows/release.yml`
- Verify you pushed the tag: `git push origin v0.1.0`

### "Pending publisher" stays pending
This is normal! The publisher will activate automatically when the first workflow successfully runs and creates the PyPI project.

---

## 🔄 Version Management Tips

**Semantic Versioning**: Follow [semver.org](https://semver.org/)
- **MAJOR**: Breaking changes (e.g., `2.0.0`)
- **MINOR**: New features, backward compatible (e.g., `1.1.0`)
- **PATCH**: Bug fixes, backward compatible (e.g., `1.0.1`)

**Recommended Workflow**:
1. Make changes in a feature branch
2. Update version in `pyproject.toml`
3. Merge to main
4. Create and push tag for automated release