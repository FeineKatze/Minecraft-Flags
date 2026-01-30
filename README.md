# Minecraft-Flags
Java Flags (and other options) for better Minecraft Performance.

## Pre-Notes
- **Disclaimer**: These work on my system (RTX 4070, Ryzen 7 5800X, 32GB DDR4 RAM); Performance may vary for you
- Only tested on Linux (Arch with **Wayland**), open an issue if something doesn't work for Windows or MacOS
- Only tested with newest Fabric on 1.21.11
- You also probably need to change some flags like `-XX:ParallelGCThreads=8` (most likely), and maybe `-XX:ReservedCodeCacheSize=512M` or similiar
- I recommend PrismLauncher as the Minecraft Launcher. It's FOSS, similiar to MultiMC and definitely better than the default launcher
- I would recommend using Java 25 (for the newest version of MC at least, 1.21.xx) and also GraalVM Enterprise (see setup below)

## Additional Setup
### GraalVM Enterprise
- Download the newest Version from this page: https://www.oracle.com/downloads/graalvm-downloads.html:
  - **Windows**: use this documentation https://docs.oracle.com/en/graalvm/jdk/25/docs/getting-started/windows/#installing-from-an-archive until step 3
  - **Linux**: use this documentation https://docs.oracle.com/en/graalvm/jdk/25/docs/getting-started/linux/#from-an-archive until step 3
  - **MacOS**: use this documentation https://docs.oracle.com/en/graalvm/jdk/25/docs/getting-started/macos/#from-an-archive until step 3
### PrismLauncher
- **Windows**: https://prismlauncher.org/download/windows/
- **Linux**: Probably in your distros repo, when not: https://prismlauncher.org/download/linux/
- **MacOS**: https://prismlauncher.org/download/macos/ or using HomeBrew: ```brew install --cask prismlauncher```

## Setup in PrismLauncher
- Create a instance and go to the **Java** tab in the instance **Settings** (or the global java settings)
- Check the checkmark for **Java Installation** <img width="114" height="21" alt="image" src="https://github.com/user-attachments/assets/d05b0d47-f54a-4508-9042-d0d66abcc388" />
- Hit **Browse** and navigate to your graalvm folder, eg. /lib/graalvm-jdk-\<version>, then go into **/bin** and select the **java** file <img width="119" height="20" alt="image" src="https://github.com/user-attachments/assets/ce95b29c-0447-4874-9734-50629651749d" /> and hit **Open**
- Also check **Skip Java compatability checks** <img width="191" height="24" alt="image" src="https://github.com/user-attachments/assets/e512ea1f-17a9-4c51-8ab4-03bcbbc9cf15" />
- For **Memory** I would recommend 8-10GB for 32GB Systems and 4-6(-8) for 16GB Systems
- Check the **Java Arguments** field and add
<details>
  <summary>Windows</summary>
  
```Java Flags
-Dgraal.TuneInlinerExploration=1 -Dgraal.CompilerConfiguration=enterprise -XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions -XX:+AlwaysPreTouch -XX:+DisableExplicitGC -XX:+UseG1GC -XX:MaxGCPauseMillis=30 -XX:G1NewSizePercent=35 -XX:G1MaxNewSizePercent=45 -XX:G1HeapRegionSize=32M -XX:G1ReservePercent=20 -XX:G1MixedGCCountTarget=4 -XX:InitiatingHeapOccupancyPercent=15 -XX:GCTimeRatio=99 -XX:+UseVectorCmov -XX:ReservedCodeCacheSize=512M -XX:+UseCodeCacheFlushing -XX:ParallelGCThreads=8 -XX:ConcGCThreads=2 -XX:MaxTenuringThreshold=1 -XX:G1HeapWastePercent=5 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:+ParallelRefProcEnabled -XX:+UseStringDeduplication -XX:+EagerJVMCI -XX:AllocatePrefetchStyle=3 -XX:-DontCompileHugeMethods -XX:MaxNodeLimit=100000 -XX:NodeLimitFudgeFactor=8000 -XX:G1ConcMarkStepDurationMillis=5.0 -XX:+AlwaysActAsServerClassMachine -XX:+UseFastUnorderedTimeStamps -XX:MaxMetaspaceSize=512M -XX:+UseCompactObjectHeaders 
```

</details>

<details>
  <summary>Linux</summary>

```Java Flags
-Dgraal.TuneInlinerExploration=1 -Dgraal.CompilerConfiguration=enterprise -XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions -XX:+AlwaysPreTouch -XX:+DisableExplicitGC -XX:+PerfDisableSharedMem -XX:+UseG1GC -XX:MaxGCPauseMillis=30 -XX:G1NewSizePercent=35 -XX:G1MaxNewSizePercent=45 -XX:G1HeapRegionSize=32M -XX:G1ReservePercent=20 -XX:G1MixedGCCountTarget=4 -XX:InitiatingHeapOccupancyPercent=15 -XX:GCTimeRatio=99 -XX:+UseNUMA -XX:+UseVectorCmov -XX:ReservedCodeCacheSize=512M -XX:+UseCodeCacheFlushing -XX:ParallelGCThreads=8 -XX:ConcGCThreads=2 -XX:MaxTenuringThreshold=1 -XX:G1HeapWastePercent=5 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:+ParallelRefProcEnabled -XX:+UseStringDeduplication -XX:+UseTransparentHugePages -XX:+EagerJVMCI -XX:AllocatePrefetchStyle=3 -XX:-DontCompileHugeMethods -XX:MaxNodeLimit=100000 -XX:NodeLimitFudgeFactor=8000 -XX:G1ConcMarkStepDurationMillis=5.0 -XX:+AlwaysActAsServerClassMachine -XX:+UseFastUnorderedTimeStamps -XX:MaxMetaspaceSize=512M -XX:+UseCompactObjectHeaders 
```
-  Also enable TransparentHugePages with
```Bash
echo madvise > /sys/kernel/mm/transparent_hugepage/enabled
```

</details>

<details>
  <summary>MacOS</summary>

```Java Flags
-Dgraal.TuneInlinerExploration=1 -Dgraal.CompilerConfiguration=enterprise -XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions -XX:+AlwaysPreTouch -XX:+DisableExplicitGC -XX:+UseG1GC -XX:MaxGCPauseMillis=30 -XX:G1NewSizePercent=35 -XX:G1MaxNewSizePercent=45 -XX:G1HeapRegionSize=32M -XX:G1ReservePercent=20 -XX:G1MixedGCCountTarget=4 -XX:InitiatingHeapOccupancyPercent=15 -XX:GCTimeRatio=99 -XX:+UseVectorCmov -XX:ReservedCodeCacheSize=512M -XX:+UseCodeCacheFlushing -XX:ParallelGCThreads=8 -XX:ConcGCThreads=2 -XX:MaxTenuringThreshold=1 -XX:G1HeapWastePercent=5 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:+ParallelRefProcEnabled -XX:+UseStringDeduplication -XX:+EagerJVMCI -XX:AllocatePrefetchStyle=3 -XX:-DontCompileHugeMethods -XX:MaxNodeLimit=100000 -XX:NodeLimitFudgeFactor=8000 -XX:G1ConcMarkStepDurationMillis=5.0 -XX:+AlwaysActAsServerClassMachine -XX:+UseFastUnorderedTimeStamps -XX:MaxMetaspaceSize=512M -XX:+UseCompactObjectHeaders 
```

</details>

## Additional Performance Boosts for Linux (maybe)
- Install FeralInteractives Gamemode (most likely in your Distros repo) and enable it in the Tweaks section and also use **gamemoderun** as a launch wrapper <img width="798" height="246" alt="image" src="https://github.com/user-attachments/assets/04baae73-17f4-45ca-b877-bbc4372d1252" />
- Also it can help if you enable the use of system libraries <img width="798" height="296" alt="image" src="https://github.com/user-attachments/assets/f53c26d5-48d0-4be9-be09-eeb7ae0ba805" />
- On older Forge (1.20.1) if you experience crashes with window creation try turning the glfw overwrite off
### Only NVIDIA
- You can also add these Env Variables in the **Environment Variable** tab (these are for **wayland**):
```Environment Variables
__GL_THREADED_OPTIMIZATIONS=1
__GLX_VENDOR_LIBRARY_NAME=nvidia
GBM_BACKEND=nvidia-drm
```
- If you have problems with a window creation error, try setting __GL_THREADED_OPTIMIZATIONS=0
