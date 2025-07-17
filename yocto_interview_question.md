# ✅ Yocto Project Interview Questions and Answers (35 Qs)

## 🟢 Basic Level

1. ❓ What is the Yocto Project?  
   → A framework for creating custom Linux distributions for embedded systems.

2. ❓ What are the main components of the Yocto Project?  
   → BitBake, Poky, OpenEmbedded, Metadata (recipes/layers), and SDK.

3. ❓ What is Poky?  
   → The reference distribution of the Yocto Project. It includes BitBake and metadata.

4. ❓ What is BitBake?  
   → BitBake is the task executor or build engine used in Yocto.

5. ❓ What is a recipe (.bb file)?  
   → It defines how to fetch, configure, build, and package software.

6. ❓ What is a layer in Yocto?  
   → A collection of related recipes, configurations, and metadata.

7. ❓ What is a machine configuration?  
   → Specifies hardware-specific settings like CPU, bootloader, and kernel.

8. ❓ What is a distro configuration?  
   → Defines high-level OS settings like init system, package format, and policies.

9. ❓ What is OpenEmbedded?  
   → A build framework used as the foundation of the Yocto Project.

10. ❓ What is IMAGE_INSTALL?  
    → A variable used to define packages included in the final image.

---

## 🟡 Intermediate Level

11. ❓ Difference between BitBake and Make?  
    → BitBake is metadata/task-based, Make is file-based.

12. ❓ What is the role of local.conf?  
    → User-specific configurations like machine type, parallel build settings.

13. ❓ What is bblayers.conf?  
    → Specifies which layers are included in the build process.

14. ❓ Common image types in Yocto?  
    → core-image-minimal, core-image-base, core-image-sato, etc.

15. ❓ How to add a new layer?  
    → Use bitbake-layers add-layer path/to/layer.

16. ❓ What is a .bbappend file?  
    → Used to modify or extend an existing recipe without editing the original.

17. ❓ How to inherit classes in recipes?  
    → Add inherit classname in your recipe (.bb file).

18. ❓ What is devtool?  
    → A command-line tool to simplify recipe development/debugging.

19. ❓ What are do_fetch, do_compile, do_install?  
    → Standard tasks: fetch source, compile code, install to target image.

20. ❓ What is a sysroot?  
    → A directory containing headers and libraries for cross-compiling.

---

## 🔴 Advanced Level

21. ❓ What is multiconfig?  
    → Allows multiple build configurations in one build environment.

22. ❓ How to create a custom image?  
    → Create a .bb file, inherit core-image, and define IMAGE_INSTALL.

23. ❓ How to enable debugging symbols?  
    → Set DEBUG_BUILD = "1" or use -g flags in EXTRA_OECONF.

24. ❓ What is PACKAGECONFIG?  
    → Used to toggle optional features in a recipe.

25. ❓ How to override variables for a machine/distro?  
    → Use MACHINE_foo = or DISTRO_foo = syntax.

26. ❓ What is PREFERRED_VERSION?  
    → Used to set a specific recipe version.

27. ❓ What is sstate-cache?  
    → Shared state cache used to reuse build outputs.

28. ❓ How to clean and rebuild a package?  
    → Run: bitbake -c clean <recipe> then bitbake <recipe>.

29. ❓ What is TMPDIR?  
    → Directory where all build outputs and intermediate files are stored.

30. ❓ How to add a custom splash screen?  
    → Write a splash recipe and include it via IMAGE_INSTALL.

---

## 🎓 Expert/Scenario-Based

31. ❓ How to integrate an external kernel or bootloader?  
    → Write a new recipe or modify linux-xyz.bb with SRC_URI and configurations.

32. ❓ How to optimize boot time?  
    → Use minimal image, disable services, INITRAMFS, compress rootfs.

33. ❓ How to enable OTA updates in Yocto?  
    → Use meta-rauc, meta-swupdate, or Mender integration.

34. ❓ How to secure a Yocto-based system?  
    → Secure boot, signed images, readonly rootfs, firewall, disable unnecessary services.

35. ❓ How to generate an SDK?  
    → bitbake <image-name> -c populate_sdk.

---

📌 Use this list to practice or prepare for interviews. Want this as a PDF or printable cheat sheet? Just ask!
