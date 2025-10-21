# 📓 Jupyter Notebook on Termux — Complete Installation Guide

Run **Jupyter Notebook** directly on your Android device using **Termux**.\
This guide covers every step from setup to fixing all common errors and dependency conflicts.

---

## ⚙️ Requirements

- Android (non-rooted)
- Latest [Termux](https://github.com/termux/termux-app/releases)
- Internet connection
- At least 2GB free storage

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
pip install virtualenv
virtualenv myenv
source myenv/bin/activate
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

## 💡 Pro Tips

- Always activate your virtualenv before running Jupyter:
  ```bash
  source ~/myenv/bin/activate
  ```
- To make Jupyter faster, use lighter themes or disable autosave.
- Use **cloudflared** or **ngrok** for remote access.
- Avoid running large notebooks; Termux memory is limited.

---

## 🎥 YouTube Guide Reference

You can use this README as the script outline for your video:

- **Part 1:** Setup Termux and dependencies
- **Part 2:** Virtual environment and Jupyter installation
- **Part 3:** Fixing all errors live
- **Part 4:** Launching and using Jupyter on Android browser

---

## 🧑‍💻 Credits

Guide authored by [Your Name or Channel Name].\
Tested on Android 13 (Realme RMX3241) using Termux v0.118.0.

---

### ✅ Final Verification Command

```bash
python -m notebook
```

If the server runs — congratulations, you have **a fully working Jupyter Notebook on Termux**!

