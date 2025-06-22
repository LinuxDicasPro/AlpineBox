# 🌄️ AlpineBox

**AlpineBox** is a simple and portable shell-based tool that allows you to create
and manage Alpine Linux **rootfs containers** using
[`proot`](https://github.com/proot-me/proot).
It enables lightweight, unprivileged environments ideal for development,
testing, and sandboxing — all **without requiring root access**.

---

## ✨ Features

* ⚡ **Fast and minimal Alpine Linux environment setup**;
* 🔒 **Runs without root** (via PRoot);
* 🧪 **Safe sandboxing** for testing or restricted systems;
* 📆 **Command execution inside containers**;
* 📁 **Support for multiple rootfs directories and caches**;
* 💪 **Ideal for compiling static binaries** using musl and Alpine's minimal toolchain.

---

## 🔧 Requirements

Make sure the following tools are available in your system:

- `coreutils (mkdir rm uname)`
- `curl`
- `find`
- `grep`
- `proot`
- `tar`

You can check dependencies using:

```bash
$ AlpineBox --check-deps
```

---

## 📦 Installation

You can install AlpineBox manually:

```bash
$ git clone https://github.com/LinuxDicasPro/AlpineBox.git
$ cd AlpineBox
$ chmod +x ./AlpineBox
$ sudo mv ./AlpineBox /usr/bin/AlpineBox
```

Or use AppImage:

```bash
$ wget https://github.com/LinuxDicasPro/AlpineBox/releases/download/Continuous/AlpineBox-1.3-x86_64.AppImage
$ mv AlpineBox-1.3-x86_64.AppImage AlpineBox
$ chmod +x ./AlpineBox
```

Then run it using:

```bash
$ AlpineBox setup
```

---

## 🚀 Usage

```bash
$ AlpineBox <command> [options] [--] [ARGS...]
```

### 📁 Commands

| Command           | Description                                             |
| ----------------- | ------------------------------------------------------- |
| `setup`           | Prepare the Alpine rootfs system (install or reinstall) |
| `config` / `conf` | Configure options before setup                          |
| `run`             | Run a command inside the rootfs using `proot`           |
| `--check-deps`    | Check whether required dependencies are installed       |
| `--help`          | Show the help message and usage guide                   |
| `-v`, `--version` | Show script version                                     |

---

## 🧰 Options

### Setup

| Option                            | Description                         |
| --------------------------------- | ----------------------------------- |
| `--cache=<DIR>` / `--cache <DIR>` | Set custom download cache directory |
| `-r`, `--reinstall`               | Force rootfs reinstallation         |
| `-d`, `--directory-rootfs <DIR>`  | Set the rootfs installation path    |
| `      --directory-rootfs=<DIR>`  | Set the rootfs installation path    |

### Config

| Option                               | Description                                            |
| ------------------------------------ | ------------------------------------------------------ |
| `-u`, `--update`                     | Run `apk update && apk upgrade` inside rootfs          |
| `-p`, `--prepare-dev`                | Install development tools (musl-dev, build-base, etc)  |
| `-e`, `--extract-static <BIN> [OUT]` | Extract the specified static binary from rootfs        |
| `      --extract-static <BIN> [OUT]` | Extract the specified static binary from rootfs        |
| `-d`, `--directory-rootfs <DIR>`     | Set the rootfs directory to operate on                 |
| `      --directory-rootfs=<DIR>`     | Set the rootfs directory to operate on                 |

### Run

| Option                           | Description                                                 |
| -------------------------------- | ----------------------------------------------------------- |
| `-0`, `--root`                   | Enable fake root mode (`proot -0`)                          |
| `-b`, `--proot-args <ARGS>`      | Additional custom PRoot arguments                           |
| `-d`, `--directory-rootfs <DIR>` | Choose a rootfs directory to run in                         |
| `      --directory-rootfs=<DIR>` | Choose a rootfs directory to run in                         |
| `-c`, `--command <CMD>`          | Execute a specific command inside the rootfs                |

---

## 🌱 Environment Variables

| Variable           | Description                                                                                |
| ------------------ | ------------------------------------------------------------------------------------------ |
| `ALPINEBOX_ROOTFS` | Default rootfs directory if not provided via `-d`. Defaults to: `~/.alpine-rootfs`         |
| `ALPINEBOX_CACHE`  | Default cache directory if not passed via `--cache`. Defaults to: `~/.cache/alpine-rootfs` |
| `ALPINEBOX_CMD`    | Command to run inside the container (alternative to `-c`, `--command`)                     |

---

## 🧪 Why AlpineBox for Static Binaries?

Alpine Linux uses the **musl libc** and provides toolchains that are
naturally geared toward **static compilation**. Combined with the
lightweight nature of AlpineBox:

* You can quickly set up isolated environments for building static binaries with `musl-gcc`;
* Perfect for creating portable binaries that run across different Linux systems;
* Avoids linking with host system libraries;
* Small footprint and fast setup, ideal for CI/CD pipelines and embedded builds.

---

## 🧪 Examples

```bash
# Setup rootfs with custom cache and directory
alpinebox setup --cache ./cache -d ./rootfs

# Run apk update inside the container using a bind mount
alpinebox run --root -d ./rootfs -c "apk update"

# Enter an interactive shell in the rootfs
alpinebox run -d ./rootfs -- /bin/sh

# Extract static binary from rootfs to ./bin
alpinebox config --extract-static proot -d ./rootfs

# Install development tools into the container
alpinebox config --prepare-dev -d ./rootfs
```

## 📄 License

This project is licensed under the [MIT License](LICENSE).
