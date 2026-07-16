# unirtos-uart-demos

[中文](README.zh.md) | English

This repository is recommended to be used through the unirtos-cli demo workflow to ensure consistency in project creation, environment setup, and build procedures.

## Feature Description

This demo demonstrates the basic development workflow of serial communication and is suitable as a beginner example for peripheral communication and debugging.

- Demonstrates UART initialization and basic send/receive paths
- Provides example verification for serial logging and data interaction
- Makes it easy to extend protocol encapsulation, buffer handling, and exception recovery logic

## Quick Start

### 1. Install the UniRTOS Toolchain

- [Development Preparation](https://www.quectel.com.cn/unirtos/docs?docs_page=快速上手/开发准备/开发准备.html)
- [Install the Cross-Compilation Toolchain](https://www.quectel.com.cn/unirtos/docs?docs_page=快速上手/环境搭建/环境搭建.html)
- [Install Python3](https://www.python.org/downloads/)
- [Install git](https://git-scm.com)
- Install `unirtos-cli`: `pip install unirtos-cli`

After the above tools are installed, confirm that the following commands are available:

```bash
python --version # Python3
git --version
unirtos --version # version 1.0.5 or later
unirtos-cli version # version 1.0.11 or later
```

### 2. Fetch the Demo Using unirtos-cli

First, check the available demos and versions:

```bash
unirtos-cli ls-demos
```

Create this demo project:

```bash
unirtos-cli new -r unirtos-uart-demos
```

To specify a version:

```bash
unirtos-cli new -r unirtos-uart-demos -v 1.0.0
```

### 3. Enter the Project and Build

```bash
cd unirtos-uart-demos-1.0.0
unirtos-cli env-setup
unirtos-cli build
```

## Common Commands

```bash
# Open the SDK menu configuration
unirtos-cli menuconfig

# Clean build artifacts
unirtos-cli clean
```

## Technical Community

Technical Community: https://forumschinese.quectel.com/c/66-category/66

## Contribution Guide

Contributions are welcome. It is recommended to submit changes in the following way:
- Run a basic verification before submission: env-setup, build, clean.
- Use clear commit messages describing the purpose of the change, scope of impact, and verification results.
- When adding new features or changing behavior, update the README and related documentation accordingly.
- Submit bug fixes and feature improvements through Issues or Pull Requests.
