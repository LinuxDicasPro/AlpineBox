<p align="center">
  <img src="logo.png" alt="AlpineBox" width="320"/>
</p>

<h1 align="center"><strong>AlpineBox - Alpine Linux SandBox</strong></h1>

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

### GLibC:
- `bash` or other shells - **100% posix**
- `coreutils (cp false mkdir rm tr true uname)`
- `findutils`
- `grep`
- `proot`
- `sed`
- `tar`
- `wget`

### Musl LibC:
- `busybox`
- `proot`

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
$ wget https://github.com/LinuxDicasPro/AlpineBox/releases/download/Continuous/AlpineBox-1.6-x86_64.AppImage
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

╔════════════════════════╗
║ Available parameters:  ║
╠════════════════════════╩═════════╦═══════════════════════
║ setup                            ║ Install/reinstall the rootfs system
║ run                              ║ Run rootfs system using proot
║ tools                            ║ Manage or build inside the rootfs
║ aports                           ║ Get APKBUILDs from the aports repository
║ --check-deps                     ║ Verify all required dependencies
║ -h, --help                       ║ Show this help message and exit
║ -v, --version                    ║ Show the script version and exit
╚══════════════════════════════════╩═══════════════════════
╔════════════════════════╗
║ Options for "setup":   ║
╠════════════════════════╩═════════╦═══════════════════════
║ -r, --reinstall                  ║ Reinstall the rootfs system
║ -n, --no-sdk                     ║ No install essential development tool
║     --edge                       ║ Use Alpine edge branch instead of stable
║     --remove-rootfs=<DIR>        ║ Delete the specified rootfs directory
║     --cache=<DIR>                ║ Set the download cache directory
║ -d, --directory-rootfs=<DIR>     ║ Set the directory for install rootfs
║     --appdir=<PACKAGE_NAME>      ║ Create rootfs in AppDir layout
╚══════════════════════════════════╩═══════════════════════
╔════════════════════════╗
║ Options for "tools":   ║
╠════════════════════════╩═════════╦═══════════════════════
║ -u, --update                     ║ Update and upgrade rootfs system
║ -e, --extract-static=<CMD> [DIR] ║ Extract static binaries from the rootfs
║ -d, --directory-rootfs=<DIR>     ║ Set the rootfs directory to be used
║ -a, --apkbuild=<APKBUILD>        ║ Compile an APKBUILD file inside the rootfs
╚══════════════════════════════════╩═══════════════════════
╔════════════════════════╗
║ Options for "aports":  ║
╠════════════════════════╩═════════╦═══════════════════════
║ -s, --search <PKG>               ║ Search for a package in the Alpine aports
║ -g, --get <PKG>                  ║ Download the APKBUILD in the Alpine aports
╚══════════════════════════════════╩═══════════════════════
╔════════════════════════╗
║ Options for "run":     ║
╠════════════════════════╩═════════╦═══════════════════════
║ -0, --root                       ║ Enables root mode in proot (uses -0)
║ -b, --proot-args=<ARGS>          ║ Passe additional arguments to proot
║ -d, --directory-rootfs=<DIR>     ║ Set the rootfs directory to be used
║ -c, --command=<CMD>              ║ Command to execute inside the environment
║ -i, --ignore-extra-binds         ║ Run without default extra binds
║ --                               ║ All arguments after this are passed to
║                                  ║ the command executed inside proot
╚══════════════════════════════════╩═══════════════════════
╔════════════════════════╗
║ Environment variables: ║
╠════════════════════════╩═════════╦═══════════════════════
║ ALPINEBOX_ROOTFS                 ║ Default rootfs directory, alternative -d/--directory-rootfs
║                                  ║ Default to: $HOME/.alpine-rootfs
║ ALPINEBOX_CACHE                  ║ Default cache directory for downloads, alternative --cache
║                                  ║ Default to: $HOME/.cache/alpine-rootfs
║ ALPINEBOX_APPDIR                 ║ Output directory for generated AppDirs
║                                  ║ Default: current directory
║ ALPINEBOX_CMD                    ║ Command to execute in the container, alternative -c/--command
╚══════════════════════════════════╩═══════════════════════
╔═══════════╗
║ Examples: ║
╠═══════════╩═════════════════════════════════════════════╗
║  $ AlpineBox setup --cache /tmp/cache -d ./rootfs       ║
║  $ AlpineBox run -0 -d ./rootfs -c "apk update"         ║
║  $ AlpineBox run -d ./rootfs -- /bin/sh                 ║
╚═════════════════════════════════════════════════════════╝

```


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
