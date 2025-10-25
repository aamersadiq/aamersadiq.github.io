---
title: "Setting Up Python in VS Code with WSL (Windows Subsystem for Linux)"
date: 2025-06-08 16:37:42
tags: [python, vscode, wsl, ubuntu, development-environment]
tags: [python, vscode, wsl, ubuntu, development-environment]
---

This guide explains how to set up a Python development environment using Visual Studio Code with Windows Subsystem for Linux (WSL). This configuration allows you to run Python scripts and Jupyter notebooks in an Ubuntu environment while using VS Code's interface.

## Why Use WSL for Python Development

Using WSL for Python development offers several advantages:

- Native Linux environment for Python libraries and tools
- No need for dual booting
- Access to both Windows and Linux tools
- Seamless integration with VS Code

## Prerequisites

Before starting, ensure you have:

- Windows 10/11 with WSL2 installed
- Ubuntu installed on WSL
- Visual Studio Code installed on Windows

## Step 1: Install VS Code Extensions

Install these extensions in VS Code:

1. Open VS Code on Windows
2. Go to Extensions (Ctrl+Shift+X)
3. Install:
   - Remote - WSL
   - Python
   - Jupyter
   - Pylance

## Step 2: Connect VS Code to Ubuntu WSL

There are two methods to connect VS Code to your Ubuntu environment:

### From VS Code

1. Click the green button in the bottom-left corner
2. Select "Connect to WSL"
3. VS Code will reopen connected to Ubuntu

### From Ubuntu Terminal

1. Open Ubuntu terminal
2. Navigate to your project directory
3. Run `code .`

## Step 3: Set Up Your Python Project

The setup files are available at: [https://github.com/aamersadiq/python-setup](https://github.com/aamersadiq/python-setup)

Clone the repository:

```bash
git clone https://github.com/aamersadiq/python-setup.git
cd python-setup
```

The repository contains:

- `setup_environment.py`: Python virtual environment setup script
- `setup_jupyter.py`: Optional Jupyter setup script
- `number_game.py`: Sample Python script
- `python_basics.ipynb`: Sample Jupyter notebook
- VS Code configuration files

## Step 4: Set Up Python Environment

1. Open a terminal in VS Code connected to WSL
2. Run:

```bash
python3 setup_environment.py
```

This script:

- Creates a Python virtual environment
- Prompts you to activate the environment
- Installs required packages
- Registers a Jupyter kernel

When prompted, activate the virtual environment:

```bash
source .venv/bin/activate
```

The script will continue automatically after activation.

## Step 5: Test Your Setup

### Running a Python Script

1. Open `number_game.py` in VS Code
2. Click the play button or press F5
3. The game runs in the terminal

### Running a Jupyter Notebook

1. Open `python_basics.ipynb` in VS Code
2. Select the Python kernel when prompted
3. Run cells using the play button or Shift+Enter

## Daily Workflow

After initial setup, your daily workflow will be:

1. Start VS Code and connect to WSL
2. Activate the virtual environment:
   ```bash
   source .venv/bin/activate
   ```
3. Work on Python files or Jupyter notebooks

## Benefits

This setup provides:

- Isolated development environments
- Consistent environment for scripts and notebooks
- VS Code's debugging and IntelliSense features
- Good performance with WSL2
- Flexibility between Windows and Linux

## Conclusion

This configuration combines VS Code's interface with Ubuntu's Python environment. The setup scripts make the process straightforward and reproducible.

For more details and all files mentioned in this guide, visit: [https://github.com/aamersadiq/python-setup](https://github.com/aamersadiq/python-setup)


