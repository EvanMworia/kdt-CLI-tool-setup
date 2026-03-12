# KDT — Windows Installation & Configuration Guide

A step-by-step guide to installing and configuring **kdt**, the open-source CLI for [Invicti ASPM](https://github.com/kondukto-io/kdt), on Windows.

---

## What is KDT?

KDT is a command line interface for **Invicti ASPM** (Application Security Posture Management). It lets you list projects and scans, trigger security scans, import results, manage SBOM files, and integrate into CI/CD pipelines — all from your terminal.

---

## Prerequisites

- A Windows machine (Windows 10 or later recommended)
- Access to an Invicti ASPM instance (host URL + API token)
- Basic familiarity with Command Prompt or PowerShell

---

## Step 1 — Download the KDT Executable

1. Go to the official KDT releases page:  
   👉 **https://github.com/kondukto-io/kdt/releases**

2. Find the **latest release** and download the file named:
    ```
    kdt-cli.exe
    ```

> This is a self-contained executable — no installer, no dependencies required.

---

## Step 2 — Place the Executable

Create a dedicated folder for the tool and move the downloaded file there.

1. Create the folder `C:\kdt` (or any location you prefer)
2. Move `kdt-cli.exe` into `C:\kdt`
3. Rename it to `kdt.exe` for convenience (optional but recommended — makes commands shorter)

Your folder should look like this:

```
C:\kdt\
  └── kdt.exe
```

---

## Step 3 — Add KDT to Your PATH

Adding the folder to your PATH means you can run `kdt` from **any terminal window**, not just from inside `C:\kdt`.

> ⚠️ Add the **folder path** (`C:\kdt`), NOT the full file path (`C:\kdt\kdt.exe`).

### How to add to PATH:

1. Press `Win + S` and search for **"Environment Variables"**
2. Click **"Edit the system environment variables"**
3. In the System Properties window, click **"Environment Variables..."**
4. Under **User variables** (top section), find and select **Path**, then click **Edit**
5. Click **New** and enter:
    ```
    C:\kdt
    ```
6. Click **OK** on all open dialogs to save

<!-- ### Verify it worked:

Open a **new** Command Prompt or PowerShell window (existing ones won't pick up the change) and run:

```
kdt --version
```

If you see a version number printed, kdt is installed and on your PATH. ✅

--- -->

## Step 4 — Set Environment Variables (Authentication)

KDT needs two things to connect to your Invicti ASPM instance: the **host URL** and an **API token**.

You can get your API token from the Invicti ASPM UI under:  
**Integrations → API Tokens**

Setting these as environment variables means you never have to type them with every command.

### How to add environment variables:

1. Follow steps 1–3 from the PATH section above to open the Environment Variables dialog
2. Under **User variables**, click **New** for each of the following:

    | Variable Name        | Value                                    |
    | -------------------- | ---------------------------------------- |
    | `INVICTI_ASPM_HOST`  | `https://your-invicti-aspm-instance.com` |
    | `INVICTI_ASPM_TOKEN` | `your_api_token_here`                    |

3. Click **OK** on all dialogs to save

### Verify the connection:

Open a **new** terminal window and run a test command:

```
kdt list projects
```

If you get a list of projects (or an empty list with no auth error), your credentials are working. ✅

> **Note:** Legacy variable names `KONDUKTO_HOST` and `KONDUKTO_TOKEN` still work but are deprecated. Use the `INVICTI_ASPM_*` names above to avoid deprecation warnings.

---

## Step 5 — Create a Config File (Optional but Recommended)

A config file is a convenient backup — especially useful if you switch between multiple environments or prefer not to rely solely on environment variables.

### Create the file:

1. Navigate to your user home directory:
    ```
    C:\Users\<YourUsername>\
    ```
2. Create a new file named exactly:
    ```
    .kdt.yml
    ```
3. Open it with any text editor (Notepad, VS Code, etc.) and add:

    ```yaml
    host: https://your-invicti-aspm-instance.com
    token: your_api_token_here
    insecure: false
    verbose: false
    ```

4. Save and close the file.

> The file must be named `.kdt.yml` (note the leading dot) and placed directly in `C:\Users\<YourUsername>\`.

### Configuration Priority

KDT reads config in this order — higher overrides lower:

```
Command line flags  >  Environment variables  >  Config file
```

So the config file acts as a safe default, and you can always override it per-command with flags.

---

## Quick Reference

Once everything is set up, here are some commands to get started:

```bash
# List all projects
kdt list projects

# List scanners available
kdt list scanners

# Override host/token for a single command
kdt --host https://other-instance.com --token mytoken list projects

# Use a custom config file
kdt --config=C:\path\to\custom-config.yaml list projects
```

---

## Troubleshooting

**`kdt` is not recognized as a command**  
→ Make sure you added `C:\kdt` (not `C:\kdt\kdt.exe`) to PATH, and that you opened a **new** terminal after making the change.

**`Host and token configuration is required` error**  
→ Check that your environment variables are named exactly `INVICTI_ASPM_HOST` and `INVICTI_ASPM_TOKEN`, and open a new terminal after setting them.

**`-t` flag not working for token**  
→ Use the full flag `--token` instead. The shorthand `-t` is not supported.

**Config file not being picked up**  
→ Confirm the file is named `.kdt.yml` (with the dot) and is located at `C:\Users\<YourUsername>\.kdt.yml`.

---

## Resources

- 📦 [KDT GitHub Repository](https://github.com/kondukto-io/kdt)
- 📥 [Latest Releases](https://github.com/kondukto-io/kdt/releases)
- 📖 [Invicti ASPM Documentation](https://www.invicti.com)
