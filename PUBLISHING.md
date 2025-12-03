## 🚀 Publishing a New Version to PyPI

Here are the updated steps to publish a new version, including the crucial information about PyPI accounts and API token authentication, which is the **recommended and secure method** for uploading packages.

-----

### 🔑 0. Prerequisites (Account & API Token)

**You must first have an account on PyPI and generate an API token for secure uploading.** Using an API token instead of your username and password is required if you have **Two-Factor Authentication (2FA)** enabled, and it is strongly recommended for all users.

1.  **Create a PyPI Account:**
      * Go to **[https://pypi.org/account/register/](https://pypi.org/account/register/)** and create a new account.
2.  **Generate an API Token:**
      * Log in, go to your **Account Settings**.
      * Navigate to the **API Tokens** section and click **Add API token**.
      * Give the token a name (e.g., "CI/CD Token" or "Manual Upload"). For the **Scope**, you can select the specific project if it already exists, or choose **Entire account (all projects)** if this is your first upload.
      * **CRITICAL:** The token value will be displayed **only once**. Copy the entire token string (it will start with `pypi-`) and save it securely.

-----

### 📦 1. Prepare and Build the Package

1.  **Update Version Number:** Update the version string in your package's metadata file, typically **`pyproject.toml`**.
2.  **Install/Upgrade Tools:** Make sure you have the latest versions of `build` and `twine`.
    ```bash
    pip install --upgrade build twine
    ```
3.  **Build the Package:** Run the build command from your project root. This creates the distribution files (source archives and wheels) in the **`dist/`** directory.
    ```bash
    python -m build
    ```

-----

### 🚀 2. Publish to PyPI with Twine

Use `twine` to upload the files from the `dist/` directory.

```bash
twine upload dist/*
```

  * **Authentication Prompt:** When you run the command above, `twine` will prompt you for API token:
    
    ```bash
    Enter your API token: 
    ```