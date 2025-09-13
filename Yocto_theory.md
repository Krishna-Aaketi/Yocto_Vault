# 🛠️ What is Yocto Project?

The **Yocto Project** is an **open-source framework** for building custom Linux distributions for embedded systems.  

It isn’t itself a Linux distribution (like Ubuntu or Fedora).  
Instead, it’s a set of **tools, metadata, and build system** (mainly **BitBake + Poky**) that lets you generate your own Linux image tailored to your hardware.

---

## 🔹 Why do we need Yocto?

Embedded devices (IoT boards, routers, automotive computers, Jetson, Raspberry Pi, etc.) often:

- Have **limited storage and RAM** → no room for a bloated OS.  
- Require **long-term support (LTS)** → same kernel/libs for 5–10 years.  
- Need **custom drivers** (WiFi, GPU, camera, etc.).  
- Must be **hardened** → remove unnecessary packages, reduce attack surface.  
- Must be **reproducible** → every build should be identical across machines and time.  

**Yocto solves these problems by:**

- Providing **recipes** (instructions) to build packages from source.  
- Producing a **minimal, optimized rootfs, kernel, and bootloader**.  
- Supporting **cross-compilation** (build on PC → run on ARM, RISC-V, PowerPC, etc.).  

---

## 🔹 Examples of Yocto Use

### 🥧 Raspberry Pi custom OS
- Instead of using Raspbian, build a lightweight Linux with only **SSH + Python + your app**.  
- Result: **faster boot**, **smaller SD card usage**, fewer updates to maintain.  

### 🤖 Jetson Orin Nano AI Box
- Build a Yocto image with **CUDA, TensorRT**, and only the services needed for AI inference.  
- No GUI, no browser, no extra bloat → **more memory and GPU cycles** for ML models.  

### 📡 Wi-Fi Router Firmware
- Projects like **OpenWrt** are based on Yocto concepts.  
- Bake in **firewall rules, VPN client, captive portal** directly into the rootfs.  

### 🚗 Automotive Dashboard (IVI)
- Car makers use Yocto (via **GENIVI / AGL**) to build infotainment OS with only:  
  - Graphics drivers  
  - Media players  
  - Bluetooth / CarPlay modules  
- No need for desktop Linux packages.  

---

✅ **In short:**  
Yocto = **your custom Linux factory** → flexible, reproducible, and optimized for embedded systems.  

# 📘 What is a Framework?

A **framework** is a set of **building blocks + rules** that helps you develop software faster.  

It provides **pre-written code, tools, and structure**, so you don’t need to start everything from scratch.  

### 🔹 Example
- Web apps → **Django (Python)** or **React (JavaScript)**  
- Embedded Linux → **Yocto Project**  

👉 Think of it like a **LEGO kit 🧱**:  
Instead of carving your own bricks, you get ready-made pieces you can arrange however you need.  

---

# 🔓 What is Open Source?

**Open source** means the source code is publicly available.  

You can **see it, modify it, and share it**, usually under licenses like **MIT, GPL, Apache**.  

### ✅ Benefits
- Free to use (no license fees)  
- Transparent (you can audit it for security)  
- Community-driven (many contributors improve it)  
- Customizable for your own project  

---

# ⚡ Open Source Framework

An **open source framework** = framework whose code is **open and free**.  

It gives you a ready-made **structure + tools**, and you can:  
- Adapt it  
- Extend it  
- Strip it down  

🔹 Backed by a **community** instead of a single vendor.  

---

# 🛠️ Examples of Open Source Frameworks
- **Django (Python web framework)** → build websites quickly  
- **TensorFlow (ML framework)** → train and run AI models  
- **ROS (Robot Operating System)** → robotics middleware framework  
- **Yocto Project** → build custom Linux distributions  
- **OpenWrt** → framework to build router firmware  

Here’s your **Yocto Components** explanation converted into Markdown format:


# 🛠️ Yocto Components

---

## 1. Poky  
**What:**  
The **reference distribution** of the Yocto Project.  

**Includes:**  
- **BitBake** (the build tool).  
- Metadata (recipes, classes, configs).  
- Example layers (`meta`, `meta-poky`, `meta-yocto-bsp`).  

**Why:**  
Provides a ready bundle so you don’t need to collect all parts manually.  

**How to use:**  
```bash
git clone git://git.yoctoproject.org/poky
cd poky
source oe-init-build-env
bitbake core-image-minimal
````

**Example:**
This builds a **minimal Linux image** using Poky.

---

## 2. BitBake

**What:**
The **task executor / build engine** (like `make`, but for Yocto).

**Why:**

* Reads recipes (`.bb`)
* Runs tasks: fetch, configure, compile, install, package
* Handles dependencies

**How to use:**

```bash
bitbake busybox
bitbake core-image-sato
```

**Example:**
`bitbake busybox` → downloads BusyBox source, compiles it for target CPU, produces a package.

---

## 3. Layers

**What:**
A **collection of related recipes and configurations** (organized like modules).

**Why:**
Keeps builds **modular & reusable**.

Examples:

* `meta-raspberrypi` → Raspberry Pi support
* `meta-tegra` → NVIDIA Jetson support
* `meta-openembedded` → extra software packages

**How to use:**

```bash
bitbake-layers add-layer ../meta-raspberrypi
```

**Example:**
Add `meta-raspberrypi`, then set:

```conf
MACHINE = "raspberrypi4-64"
```

---

## 4. Recipes (`.bb` files)

**What:**
Instructions for building software.

**Why:**
Defines how to:

* Fetch source code (git, tarball)
* Apply patches
* Configure (CMake, Autotools)
* Compile
* Install into rootfs

**How to use:**
Recipes live inside layers, e.g.

```
meta-oe/recipes-core/busybox/busybox_1.35.bb
```

**Example (simple Hello recipe):**

```bitbake
SUMMARY = "Hello World text file"
LICENSE = "MIT"
SRC_URI = "file://hello.txt"

do_install() {
    install -d ${D}${datadir}/myapp
    install -m 0644 ${WORKDIR}/hello.txt ${D}${datadir}/myapp/hello.txt
}
```

This installs `hello.txt` into `/usr/share/myapp/`.

---

## 5. Images

**What:**
Special recipes that define **what packages go into the final rootfs**.

**Why:**

* Control **system size & features**.
* Select only needed packages.

**How to use:**

```bash
bitbake core-image-minimal
bitbake core-image-sato   # GUI version
```

**Example (custom image):**

```bitbake
DESCRIPTION = "My Custom IoT Image"
LICENSE = "MIT"
IMAGE_INSTALL = "packagegroup-core-boot busybox dropbear python3"
inherit core-image
```

This builds an image with **BusyBox, SSH (Dropbear), and Python3**.

---

## 6. SDKs (Software Development Kits)

**What:**
Cross-compilation toolchains generated by Yocto.

**Why:**

* App developers can build software **without installing full Yocto**.
* Provides **sysroot, headers, and cross-compiler**.

**How to use:**

Generate SDK:

```bash
bitbake core-image-minimal -c populate_sdk
```

Install SDK:

```bash
./tmp/deploy/sdk/poky-glibc-x86_64-core-image-minimal-aarch64-toolchain.sh
```

Setup environment:

```bash
source /opt/poky/3.1/environment-setup-aarch64-poky-linux
```

Compile app for ARM:

```bash
gcc main.c -o main
```

---

# ✅ Summary

* **Poky** → The reference build system (your starting kit).
* **BitBake** → The engine that executes recipes.
* **Layers** → Organized sets of recipes (like plugins).
* **Recipes** → Instructions for building software packages.
* **Images** → Define what goes into the final Linux distro.
* **SDKs** → Toolchains for developers to build apps without full Yocto.


# 🔹 Yocto Build Flow

---

## 1️⃣ Setup Environment (`oe-init-build-env`)

**What it is:**

* A script inside **Poky** that prepares your build environment.
* Creates a `build/` directory with default configs.
* Sets environment variables so `bitbake` works.

**Why:**

* You need a clean sandbox for every Yocto build.
* Keeps multiple builds (e.g., for QEMU, Raspberry Pi, Jetson) separated.

**How to use:**

```bash
cd poky
source oe-init-build-env
```

👉 After this, you’ll be inside `poky/build/` with files:

* `conf/local.conf` → local build config
* `conf/bblayers.conf` → list of active layers

**Example analogy:** Like opening a new project workspace in VS Code.

---

## 2️⃣ Configure (`local.conf`, `bblayers.conf`)

**What it is:**

* **`local.conf`** → per-build settings (machine, parallelism, packages).
* **`bblayers.conf`** → which layers (meta-\*) are included.

**Why:**

* Controls *what hardware you’re targeting* and *what recipes are available*.
* Customizes how heavy/light your image will be.

**How to use:**
Edit `conf/local.conf`:

```conf
MACHINE ?= "qemux86-64"      # target hardware (QEMU emulated x86-64)
BB_NUMBER_THREADS = "4"      # how many CPU threads to use
PARALLEL_MAKE = "-j 4"       # parallel make jobs
IMAGE_INSTALL:append = " python3"   # add extra packages
```

Edit `conf/bblayers.conf`:

```conf
BBLAYERS ?= " \
  /home/user/poky/meta \
  /home/user/poky/meta-poky \
  /home/user/poky/meta-yocto-bsp \
"
```

**Example analogy:** Like choosing which VS Code extensions and settings your project will use.

---

## 3️⃣ Run a Build (`bitbake core-image-minimal`)

**What it is:**

* `bitbake` reads recipes and builds packages + images.
* Example image recipes:

  * `core-image-minimal` → tiny OS (shell only).
  * `core-image-sato` → GUI desktop (X11 + GTK).
  * `core-image-base` → small but with more utilities.

**Why:**

* Produces the **kernel, rootfs, bootloader, and packages**.
* Everything is **cross-compiled** for your target architecture.

**How to use:**

```bash
bitbake core-image-minimal
```

👉 First build takes **hours** (downloads + builds toolchain + all packages).
👉 Later builds are faster (uses sstate-cache).

**Output:**
`tmp/deploy/images/<machine>/`
Contains kernel (`bzImage`), rootfs (`.ext4`, `.tar.gz`), bootloader, etc.

**Example analogy:** Like pressing **Build & Run** in VS Code → compiler produces binaries.

---

## 4️⃣ Deploy & Run (QEMU or Hardware)

**What it is:**

* Running the built image.
* Either in **QEMU emulator** (for testing) or **flashed to hardware** (e.g., Raspberry Pi, Jetson).

**Why:**

* Validate your image before shipping to real device.
* Saves time & debugging cost.

**How to use (QEMU):**

```bash
runqemu qemux86-64
```

This boots the built image in a virtual machine.

**How to use (Hardware, e.g. SD card for Raspberry Pi):**

```bash
dd if=tmp/deploy/images/raspberrypi4/core-image-minimal-raspberrypi4.rootfs.wic \
   of=/dev/sdX bs=4M
sync
```

Then insert SD into Raspberry Pi → boot.

**Example analogy:** Like running your program either in a simulator (QEMU) or on the real robot (Jetson).

---

# ✅ Summary Flow

1. **Setup environment** → prepare `build/` sandbox.
2. **Configure** → edit `local.conf` (machine, options), add layers.
3. **Run build** → `bitbake <image>` compiles everything.
4. **Deploy & run** → test in QEMU or flash to real hardware.


---

# 0) Prereqs (Ubuntu 22.04+ on x86\_64)

```bash
sudo apt update
sudo apt install -y gawk wget git-core diffstat unzip texinfo gcc \
  build-essential chrpath socat cpio python3 python3-pip python3-pexpect \
  xz-utils debianutils iputils-ping python3-git python3-jinja2 xterm \
  device-tree-compiler bmap-tools
mkdir -p ~/yocto && cd ~/yocto
```

---

# 1) Build a minimal Linux for QEMU

## 1.1 Get Poky (Yocto reference)

```bash
git clone https://git.yoctoproject.org/poky
cd poky
git checkout scarthgap       # stable LTS series
```

## 1.2 Init the build env (creates `build/`)

```bash
source oe-init-build-env     # drops you into poky/build
```

## 1.3 Configure basics (`conf/local.conf`)

Set target and speed up rebuilds (optional cache dirs):

```bash
sed -i 's/^MACHINE.*/MACHINE ?= "qemux86-64"/' conf/local.conf
cat >> conf/local.conf << 'EOF'
BB_NUMBER_THREADS = "4"
PARALLEL_MAKE = "-j4"
DL_DIR ?= "${TOPDIR}/../downloads"
SSTATE_DIR ?= "${TOPDIR}/../sstate-cache"
EOF
```

## 1.4 Build & run

```bash
bitbake core-image-minimal
runqemu qemux86-64
```

In the VM, log in as `root` (usually no password).
Exit QEMU with `Ctrl+A` then `X` (on serial) or close the window.

---

# 2) Create your own layer (`meta-myproject`)

## 2.1 Create & add

```bash
cd ~/yocto/poky/build
bitbake-layers create-layer ../meta-myproject
bitbake-layers add-layer ../meta-myproject
bitbake-layers show-layers   # verify it’s listed
```

The tool created `../meta-myproject/conf/layer.conf` for you.

---

# 3) Add a tiny custom recipe (Hello app)

We’ll compile a small C program into `/usr/bin/my-hello`.

## 3.1 Files & recipe layout

```bash
mkdir -p ../meta-myproject/recipes-myproject/my-hello/files
```

Create **`../meta-myproject/recipes-myproject/my-hello/files/hello.c`**:

```c
#include <stdio.h>
int main(){ puts("Hello from Yocto!"); return 0; }
```

Create **`../meta-myproject/recipes-myproject/my-hello/my-hello_1.0.bb`**:

```bitbake
SUMMARY = "Very small hello app"
LICENSE = "MIT"
LIC_FILES_CHKSUM = "file://${COMMON_LICENSE_DIR}/MIT;md5=0835ade698e0bcf8506ecda2f7b4f302"

SRC_URI = "file://hello.c"
S = "${WORKDIR}"

TARGET_CC_ARCH += "${LDFLAGS}"  # ensure correct target flags

do_compile() {
    ${CC} ${CFLAGS} ${LDFLAGS} hello.c -o my-hello
}

do_install() {
    install -d ${D}${bindir}
    install -m 0755 my-hello ${D}${bindir}/my-hello
}
```

## 3.2 Build the package

```bash
bitbake my-hello
```

(You’ll find the built package under `tmp/deploy/ipk`/`rpm`/`deb` depending on your PACKAGE\_CLASSES.)

---

# 4) Customize an image (include your app + useful tools)

Two common ways:

## A) Quick way: append to `local.conf`

```bash
echo 'IMAGE_INSTALL:append = " my-hello bash python3 coreutils"' >> conf/local.conf
bitbake core-image-minimal
runqemu qemux86-64
```

In QEMU:

```bash
my-hello
python3 -V
```

## B) Proper way: make your own image recipe

Create **`../meta-myproject/recipes-myproject/images/my-image.bb`**:

```bitbake
DESCRIPTION = "My custom lab image"
LICENSE = "MIT"

inherit core-image

# Add nice features; debug-tweaks = root login w/o password on console
EXTRA_IMAGE_FEATURES += "debug-tweaks ssh-server-dropbear"

# Add packages you want in rootfs
IMAGE_INSTALL:append = " my-hello bash python3 vim iproute2"

# (Optional) Smaller image by stripping locales/docs, etc., via DISTRO_FEATURES tweaks.
```

Build & run your image:

```bash
bitbake my-image
runqemu qemux86-64 nographic
# login: root
my-hello
```

---

## Useful checks & tips

* Show active layers: `bitbake-layers show-layers`
* Find where an image/recipe lives: `bitbake -e my-image | grep ^FILE=`
* Clean & rebuild one thing: `bitbake -c cleansstate my-hello && bitbake my-hello`
* Speed: keep `DL_DIR`/`SSTATE_DIR` outside `build/` (already set above)
* Change MACHINE later (e.g., `qemuarm64`) by editing `local.conf` and rebuilding.

---

### You’re done 🎉

You built a QEMU image, created a layer, added your own app, and customized the image.
