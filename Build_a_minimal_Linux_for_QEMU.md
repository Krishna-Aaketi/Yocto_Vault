
### Commands:-
```
sudo apt update
sudo apt install -y gawk wget git-core diffstat unzip texinfo gcc \
  build-essential chrpath socat cpio python3 python3-pip python3-pexpect \
  xz-utils debianutils iputils-ping python3-git python3-jinja2 xterm \
  device-tree-compiler bmap-tools
```
### command:- git clone https://git.yoctoproject.org/poky
#### output
```
Cloning into 'poky'...
remote: Enumerating objects: 702792, done.
remote: Counting objects: 100% (5755/5755), done.
remote: Compressing objects: 100% (1444/1444), done.
remote: Total 702792 (delta 4710), reused 4860 (delta 4283), pack-reused 697037 (from 1)
Receiving objects: 100% (702792/702792), 220.50 MiB | 3.09 MiB/s, done.
Resolving deltas: 100% (510727/510727), done.
```
### Commands:-
```
cd poky/
ls
```
### Command:- git checkout scarthgap
#### Output
```
branch 'scarthgap' set up to track 'origin/scarthgap'.
Switched to a new branch 'scarthgap'
```
### Command:- source oe-init-build-env
#### Output
```
You had no conf/local.conf file. This configuration file has therefore been
created for you from /home/scl-pvt-ltd/krishna_yocto/poky/meta-poky/conf/templates/default/local.conf.sample
You may wish to edit it to, for example, select a different MACHINE (target
hardware).

You had no conf/bblayers.conf file. This configuration file has therefore been
created for you from /home/scl-pvt-ltd/krishna_yocto/poky/meta-poky/conf/templates/default/bblayers.conf.sample
To add additional metadata layers into your configuration please add entries
to conf/bblayers.conf.

The Yocto Project has extensive documentation about OE including a reference
manual which can be found at:
    https://docs.yoctoproject.org

For more information about OpenEmbedded see the website:
    https://www.openembedded.org/

This is the default build configuration for the Poky reference distribution.

### Shell environment set up for builds. ###

You can now run 'bitbake <target>'

Common targets are:
    core-image-minimal
    core-image-full-cmdline
    core-image-sato
    core-image-weston
    meta-toolchain
    meta-ide-support

You can also run generated qemu images with a command like 'runqemu qemux86-64'.

Other commonly useful commands are:
 - 'devtool' and 'recipetool' handle common recipe tasks
 - 'bitbake-layers' handles common layer tasks
 - 'oe-pkgdata-util' handles common target package tasks
You had no /home/scl-pvt-ltd/krishna_yocto/poky/.vscode configuration.
These configuration files have therefore been created for you.

```
### Commands:- 
```
sed -i 's/^MACHINE.*/MACHINE ?= "qemux86-64"/' conf/local.conf
cat >> conf/local.conf << 'EOF'
BB_NUMBER_THREADS = "4"
PARALLEL_MAKE = "-j4"
DL_DIR ?= "${TOPDIR}/../downloads"
SSTATE_DIR ?= "${TOPDIR}/../sstate-cache"
EOF
```
### Command:- bitbake core-image-minimal

#### Output
```
ERROR: User namespaces are not usable by BitBake, possibly due to AppArmor.
See https://discourse.ubuntu.com/t/ubuntu-24-04-lts-noble-numbat-release-notes/39890#unprivileged-user-namespace-restrictions for more information.

Summary: There was 1 ERROR message, returning a non-zero exit code.

```
## Error Solution Steps:-
### Command:- sudo sysctl -w kernel.apparmor_restrict_unprivileged_userns=0
#### Output
```
[sudo] password for scl-pvt-ltd: 
kernel.apparmor_restrict_unprivileged_userns = 0
```
### Command:- echo "kernel.apparmor_restrict_unprivileged_userns=0" |   sudo tee /etc/sysctl.d/99-yocto.conf
#### Output
```
kernel.apparmor_restrict_unprivileged_userns=0
```
### Commands:- sudo sysctl --system
#### Output
```
* Applying /usr/lib/sysctl.d/10-apparmor.conf ...
* Applying /etc/sysctl.d/10-bufferbloat.conf ...
* Applying /etc/sysctl.d/10-console-messages.conf ...
* Applying /etc/sysctl.d/10-ipv6-privacy.conf ...
* Applying /etc/sysctl.d/10-kernel-hardening.conf ...
* Applying /etc/sysctl.d/10-magic-sysrq.conf ...
* Applying /etc/sysctl.d/10-map-count.conf ...
* Applying /etc/sysctl.d/10-network-security.conf ...
* Applying /etc/sysctl.d/10-ptrace.conf ...
* Applying /etc/sysctl.d/10-zeropage.conf ...
* Applying /usr/lib/sysctl.d/30-tracker.conf ...
* Applying /usr/lib/sysctl.d/50-bubblewrap.conf ...
* Applying /usr/lib/sysctl.d/50-pid-max.conf ...
* Applying /usr/lib/sysctl.d/99-protect-links.conf ...
* Applying /etc/sysctl.d/99-sysctl.conf ...
* Applying /etc/sysctl.d/99-yocto.conf ...
* Applying /etc/sysctl.conf ...
kernel.apparmor_restrict_unprivileged_userns = 1
net.core.default_qdisc = fq_codel
kernel.printk = 4 4 1 7
net.ipv6.conf.all.use_tempaddr = 2
net.ipv6.conf.default.use_tempaddr = 2
kernel.kptr_restrict = 1
kernel.sysrq = 176
vm.max_map_count = 1048576
net.ipv4.conf.default.rp_filter = 2
net.ipv4.conf.all.rp_filter = 2
kernel.yama.ptrace_scope = 1
vm.mmap_min_addr = 65536
fs.inotify.max_user_watches = 65536
kernel.unprivileged_userns_clone = 1
kernel.pid_max = 4194304
fs.protected_fifos = 1
fs.protected_hardlinks = 1
fs.protected_regular = 2
fs.protected_symlinks = 1
kernel.apparmor_restrict_unprivileged_userns = 0
```
### Command:- bitbake core-image-minimal
#### Output
```

```
