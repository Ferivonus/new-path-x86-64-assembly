# 🛠️ Setting Up NASM Assembly Development on Windows with VSCode

This guide explains how to set up a basic NASM (x86-64) assembly development environment on Windows using Visual Studio Code. I had to piece this together from different sources, so I wanted to share a complete step-by-step tutorial that might help others too.

---

## 📦 Prerequisites: Installing Package Managers

We'll use **Chocolatey** and **Scoop** to install the tools easily. If you don't already have them, here’s how to install each:

### ✅ Install Chocolatey (if you don’t have it):

1. Open PowerShell **as Administrator**.
2. Run this command:

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; `
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; `
iwr https://community.chocolatey.org/install.ps1 -UseBasicParsing | iex
```

### ✅ Install Scoop (if you don’t have it):

1. Open **PowerShell (not as Administrator)**.
2. Run:

```powershell
iwr -useb get.scoop.sh | iex
```

---

## 🔧 Step 1: Install NASM and MinGW

Now that we have the package managers:

```bash
choco install nasm
scoop install mingw
```

* `nasm` is the assembler.
* `mingw` gives you `gcc`, which is used to link the object file into an `.exe`.

---

## 💻 Step 2: Set Up VSCode

### 🧩 Recommended Extension

For better syntax highlighting, install this in VSCode:
[ARM Assembly - dan-c-underwood](https://marketplace.visualstudio.com/items?itemName=dan-c-underwood.arm)

It works well with x86-style syntax too.

### 📁 Create Project Folder

1. Create a new folder for your project, e.g., `nasm-vscode-example`
2. Inside it, create a folder named `.vscode`

### 🧱 Create `tasks.json` for Build

In `.vscode`, create a `tasks.json` file:

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Build All",
      "type": "shell",
      "dependsOn": ["Assemble", "Link"],
      "group": {
        "kind": "build",
        "isDefault": true
      }
    },
    {
      "label": "Assemble",
      "type": "shell",
      "command": "nasm",
      "args": [
        "-f", "win64",
        "main.asm",
        "-o", "main.obj"
      ]
    },
    {
      "label": "Link",
      "type": "shell",
      "command": "gcc",
      "args": [
        "main.obj",
        "-o",
        "main.exe"
      ]
    }
  ]
}
```

### 🐞 Create `launch.json` for Debugging

Also in `.vscode`, add a `launch.json` file:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "lldb",
      "request": "launch",
      "name": "Debug",
      "program": "${workspaceFolder}/main.exe",
      "args": [],
      "cwd": "${workspaceFolder}"
    }
  ]
}
```

---

## 🚀 Usage Instructions

1. Create your `main.asm` in the project root folder.
2. To build the project, press `Ctrl + Shift + B`.
3. To debug/run it, press `F5`.

---

## 🙌 Need Help?

If something’s not working or you get stuck, feel free to open an issue or discussion in this repo. I’m sharing this to help others who might be trying to get into low-level programming on Windows without getting lost in the setup process.

Happy hacking! ⚙️
