# 📓 Jupyter Notebook on Termux — Complete Installation Guide

Run **Jupyter Notebook** directly on your Android device using **Termux**!

This comprehensive guide covers everything you need to know about setting up and running Jupyter Notebooks on your Android device, from initial installation to advanced usage and troubleshooting.

## 📑 Table of Contents
- [Requirements](#%EF%B8%8F-requirements)
- [Installation Steps](#-step-1-update-and-upgrade)
- [Configuration](#-configuration)
- [Usage Guide](#-usage-guide)
- [Troubleshooting](#-common-issues-and-fixes)
- [Pro Tips](#-pro-tips)
- [Credits](#-credits)

---

## ⚙️ Requirements

### System Requirements
- Android device (non-rooted, Android 7.0 or higher recommended)
- At least 2GB free storage space
- 1GB or more RAM recommended
- Active internet connection

### Software Requirements
- Latest [Termux](https://github.com/termux/termux-app/releases) from the official GitHub repository
- (Optional) Any modern web browser for accessing notebooks

### Recommended Add-ons
- [Termux:API](https://wiki.termux.com/wiki/Termux:API) - For additional functionality
- [Hacker's Keyboard](https://play.google.com/store/apps/details?id=org.pocketworkstation.pckeyboard) - For better coding experience

---

## 🚀 Step 1: Update and Upgrade

Always start with a clean base.

```bash
pkg update -y && pkg upgrade -y
```

---

## 🧱 Step 2: Install Essential Build Tools

You need these for compiling Python packages and C extensions.

```bash
pkg install -y python rust libffi clang make cmake git libbz2 zlib libjpeg-turbo
```

---

## 🧰 Step 3: Set Up a Python Virtual Environment

It’s safer to isolate your Jupyter installation.

```bash
# Bash
python -m venv --system-site-packages ~/myenv
source ~/myenv/bin/activate

# Fish
python -m venv --system-site-packages ~/myenv
source ~/myenv/bin/activate.fish
```

To exit later:

```bash
deactivate
```

---

## 🧩 Step 4: Upgrade Python Packaging Tools

```bash
pip install --upgrade pip setuptools wheel
```

---

## 🧠 Step 5: Fix Common Header or Compiler Errors

If you see:

```
fatal error: ffi.h: No such file or directory
```

Then make sure `libffi` is installed:

```bash
pkg install -y libffi
```

If you get:

```
configure: error: no acceptable ld found in $PATH
```

Then install:

```bash
pkg install -y binutils
```

If you see:

```
E: Unable to correct problems, you have held broken packages.
```

Then run:

```bash
pkg clean
pkg autoclean
pkg update && pkg upgrade -y
```

---

## 🧬 Step 6: Rebuild cffi (Critical for Jupyter Dependencies)

```bash
CFLAGS="-I$PREFIX/include" LDFLAGS="-L$PREFIX/lib" pip install --no-cache-dir --force-reinstall cffi
```

---

## 📦 Step 7: Install Jupyter Notebook

```bash
pip install --no-cache-dir jupyter
```

---

## ▶️ Step 8: Run Jupyter Notebook

```bash
jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser
```

You’ll see output like:

```
http://127.0.0.1:8888/?token=abcdef123456
```

Copy the token URL and open it in your Android browser.

---

## 🌐 (Optional) Access Jupyter from Another Device

If you want to use it on your PC, you can expose it using [Cloudflared](https://github.com/cloudflare/cloudflared):

```bash
pkg install cloudflared
cloudflared tunnel --url http://localhost:8888
```

This will give you a public HTTPS URL.

---

## 🧹 Common Issues and Fixes

| Error                    | Cause                       | Fix                                      |
| ------------------------ | --------------------------- | ---------------------------------------- |
| `ffi.h missing`          | libffi not installed        | `pkg install libffi`                     |
| `no acceptable ld found` | binutils missing            | `pkg install binutils`                   |
| `held broken packages`   | outdated repo data          | `pkg clean && pkg update && pkg upgrade` |
| `rust not found`         | missing Rust compiler       | `pkg install rust`                       |
| `maturin build failed`   | missing setuptools or wheel | `pip install --upgrade setuptools wheel` |

---

## 🧾 Example Verification

```bash
python -m notebook --version
```

If it prints a version (e.g. `7.2.1`), you’re good to go.

---
## 😎Create Quick Start Scripts and Run Jupyter Notebook
```bash
# Bash: start_jupyter.sh
#!/data/data/com.termux/files/usr/bin/bash
source ~/myenv/bin/activate
jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser --NotebookApp.token='' --NotebookApp.password=''

# Fish: start_jupyter.fish
#!/data/data/com.termux/files/usr/bin/fish
source ~/myenv/bin/activate.fish
jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser --NotebookApp.token='' --NotebookApp.password=''

# Make scripts executable
chmod +x start_jupyter.sh
chmod +x start_jupyter.fish

# Run Jupyter Notebook
# Bash
./start_jupyter.sh
# Fish
./start_jupyter.fish
```
## 💡 Pro Tips

### Performance Optimization
- Always activate your virtualenv before running Jupyter:
  ```bash
  source ~/myenv/bin/activate
  ```
- Use lighter themes or disable autosave for better performance
- Clear notebook output cells when saving to reduce file size
- Avoid running resource-intensive computations
- Consider using lightweight alternatives to heavy libraries

### Remote Access
- Use **cloudflared** for secure HTTPS tunneling
- **ngrok** provides an alternative for remote access
- Consider setting up SSH access for remote terminal sessions
- Use port forwarding for local network access

### Security Best Practices
- Always set a strong notebook password
- Don't expose notebooks to the public internet without authentication
- Regularly update all packages and dependencies
- Back up your notebooks regularly

### Memory Management
- Avoid running large notebooks; Termux memory is limited
- Monitor memory usage with `top` or `htop`
- Clear kernel memory by restarting kernels periodically
- Use `%%capture` magic to prevent output memory buildup

### Editor Tips
- Install a coding-friendly keyboard like Hacker's Keyboard
- Use keyboard shortcuts for efficient navigation
- Consider using JupyterLab for a more IDE-like experience
- Enable line numbers for easier code reference

---
##  Testing and Verification

### Basic Testing
```bash
# Check Python installation
python --version

# Verify Jupyter installation
jupyter --version

# Test notebook server
python -m notebook
```

### Advanced Testing
1. Test markdown rendering
2. Verify code completion
3. Check package imports
4. Test plot generation
5. Verify file saving/loading

## 📱 Compatible Devices

Successfully tested on:
- Android 13 (Realme RMX3241)
## 🧑‍💻 Credits

### Author
Guide authored and maintained by AKSHAY.

### Version Information
- Document Version: 2.0.0
- Last Updated: October 2025
- Tested with:
  - Termux v0.118.0
  - Python 3.10+
  - Jupyter Notebook 7.0+

### Contributing
Found an issue or want to contribute? Please visit our [GitHub repository](https://github.com/AKSHAY355-a/Jupyter-notebook-on-Android-TERMUX-) or submit a pull request.
