# Windows 7/10/11 Optimization Guide

**Reviewed: 22 December 2024**  
**Scope:** Windows 7/10/11 desktops and laptops. Some menu names differ by Win version/edition.

> **NOTE:** removed the `.VBS` SCRIPT FILE HAS BECAUSE IT STARTED CAUSING ISSUES ON WINDOWS 10 and was not working in 11. WILL REVIEW AND UPDATE IT SEPARATELY.

New guide combines Win cleanup, optimization, network tuning, driver maintenance, BIOS checks + some advanced troubleshooting into one ordered workflow. The VBS was doing all random, now the md is designed to be followed from top to bottom way, but NOT every tweak needed to be applied to every computer. Made a priority table & impact table to decide what is worth changing on your hardware after seeing any changes.

## Read this first (added Dec 2025)

- Windows 10 standard support ended on **14 October 2025**... so System Security related tweaks are not too safe... so removed some of them...
- Create a restore point before changing system settings. A restore point is useful for rolling back system configuration, but it is **not** a backup of personal files.
- Pls do benchmark before & after. A tweak is NOT an improvement, UNLESS it produces a repeatable result on your machine.

## Priority + expected impact - table

**Priority:** P0 = preparation/safety; P1 = best first changes; P2 = situational, test on your hardware; P3 = advanced or experimental.  
**Impact** is the likely practical benefit, not a guaranteed FPS percentage. Storage cleanup mostly helps when the drive is nearly full; it does not magically make a healthy PC faster.

| Priority | Tweak / area | Expected impact | Edit / recommended action | Risk / limitation |
|---|---|---|---|---|
| P0 | Baseline | High diagnostic value | Record boot time, free space, CPU/RAM/disk use, FPS/frame times, ping, jitter, packet loss and internet speed before changes | No direct speed gain; essential for comparison |
| P0 | System Restore | High recovery value | Enable protection, set a 3–5% allocation if suitable, remove old points only if needed, then create a fresh named point | Delete old points + remove rollback history |
| P1 | Windows updates and official drivers | Medium; sometimes high when fixing a bug | Install supported Windows updates and correct chipset, LAN/Wi-Fi and GPU drivers | A newest driver is not always the most stable; retain rollback option |
| P1 | Startup apps + unused software | Medium to high on cluttered systems | Disable nonessential startup apps; uninstall software you recognize and do not use | Disabling hardware/security components can break features |
| P1 | Disk cleanup/Storage Sense | High for a nearly full drive; otherwise mostly storage recovery | Remove selected temporary/update files; enable Storage Sense carefully | Review Downloads, Recycle Bin and previous Windows installation before deletion |
| P1 | Visual effects + background apps | Low to medium; may improve perceived responsiveness | Reduce animations/transparency; close or restrict apps that truly run in the background | Small gains on powerful PCs; personal usability trade-off |
| P1 | Game Mode | Low to medium; game-dependent | Enable Game Mode; select High performance GPU for a game on a hybrid-GPU laptop | Not a guaranteed FPS increase |
| P1 | Drive optimization | Medium when an HDD is fragmented; routine maintenance on SSD | Use Windows Optimize Drives; HDD gets defrag, SSD gets TRIM/optimization | Do not use third-party forced defrag on SSDs |
| P2 | GPU control-panel settings | Medium in particular games | Tune per-game settings; use Reflex/Anti-Lag where supported | Global overrides can reduce image quality or cause stutter |
| P2 | Network adapter/power features | Low to medium if they cause link drops or latency variation | Test Energy Efficient Ethernet/Green Ethernet and device power management only when symptoms exist | More power use; Wi-Fi battery life may decrease |
| P2 | DNS selection | Low; affects name lookups, not game-server ping | Compare ISP DNS with Google/Cloudflare or a reputable benchmark | Does not raise bandwidth or reduce an established game's route latency |
| P2 | Cache reset (browser, Store, GPU, & apps if u have, like spotify) | Low to medium for troubleshooting or freeing space | Clear the specific cache only when large, corrupt, or causing a problem | First launch may be slower while cache rebuilds |
| P2 | BIOS XMP/EXPO and Resizable BAR | Potentially medium or high on a compatible system | Enable only supported features; validate stability after each change | Instability, boot loops, or data corruption if overclock is unstable |
| P3 | Registry and TCP Optimizer presets | Uncertain / often negligible | Keep defaults unless solving a measured issue; export registry and save original network settings | Can make latency, throughput, audio, or networking worse |
| P3 | Core parking, timer resolution, Process Lasso, affinities | Usually low / workload-specific | Test one change at a time and measure frame times | More heat/power, scheduling regressions, or unstable behavior |
| P3 | Disable Memory Integrity/VBS, Defender, UAC, Secure Boot | No universal benefit; security cost can be substantial | Keep security protections enabled unless a measured compatibility problem justifies a deliberate test | Reduces protection against malicious drivers and kernel attacks |

---

# PART A — PREPARATION AND BASELINE

## 1. DO benchmark your current system first

Do not start changing settings without recording how the computer performs RIGHT now. So get a brief idea of whats happening to ur machine....

### System baseline

1. Open Task Manager by `Ctrl + Shift + Esc`.
2. Check **Processes**, **Performance**, & **Startup apps**.
3. See ur CPU, memory, disk & GPU usage while idle and while running your usual game/application.
4. Record available space on `C:` and other drives.
5. Record normal boot-to-desktop time and the time taken to launch your usual applications.
6. For games, record average FPS, 1% lows if available, frame-time consistency, temperature and GPU/CPU utilization. Use the same game, scene, graphics settings and measurement period after changes...

### Internet baseline

1. Run a test at [Speedtest.net](https://www.speedtest.net/) or another reputable speed-test service.
2. Run **2–5 tests** and record download speed, upload speed, idle latency, loaded latency, jitter and packet loss where available.
3. If your ISP/router supports it, use its router-to-device test (for example, SamKnows RealSpeed) to compare the speed entering the router with the speed reaching the PC. Availability depends on the ISP and router.
4. For gaming, record ping to the actual game region/server. A generic speed test does not measure the route to every game server.
5. Repeat tests after changes using the same device, connection type, test server and approximate conditions.

**Why this?** Cuz it helps distinguish PC problem from a Wi-Fi/router/ISP/location problem.

## 2. Update Windows and install the correct drivers

### Windows Update

- Windows 11: `Settings > Windows Update > Check for updates`.
- Windows 10: `Settings > Update & Security > Windows Update > Check for updates`.
- Restart when required. Review optional driver updates rather than blindly installing every optional item.

**Windows 10 users:** standard support ended on 14 October 2025. Check Windows 10 22H2 ESU eligibility/enrollment or plan a move to supported Windows. ESU provides eligible security updates, not a return to normal feature support.

### Driver order

1. **Laptop / motherboard drivers:** Check the exact model's official support page for chipset, platform, storage, Wi-Fi, Ethernet, Bluetooth and audio drivers.
2. **Network adapter:** Identify the active adapter in Device Manager or `Control Panel > Network and Internet > Network Connections`. Use the correct Realtek, Intel, Killer or OEM package for that exact adapter and Windows version.
3. **GPU:** Download drivers from the official NVIDIA, AMD or Intel graphics support page. Use the correct driver for the exact GPU and Windows version.
4. **Peripheral drivers:** Update mouse, keyboard, controller, headset and monitor software only when needed.

Do not use Driver Booster or an unknown driver updater as the default method. Device Manager's generic search is not guaranteed to find the newest vendor driver. Do not install a driver simply because its version number is newer; stability and OEM compatibility matter.

### Clean GPU driver installation

A normal official driver update is sufficient for most users. Use Display Driver Uninstaller (DDU) only when a GPU driver installation is corrupted, a persistent driver issue remains, or you are switching GPU vendors. Download DDU only from its official source, follow its instructions, and create a restore point first. Do not use DDU routinely before every driver update.

---

# PART B — RESTORE POINT AND STORAGE CLEANUP

## 3. Create a restore point before tweaking

1. Press `Win + R`.
2. Run:

```text
SystemPropertiesProtection.exe
```

3. Select the Windows drive, usually `C:`, and click **Configure**.
4. Enable **Turn on system protection**.
5. Set **Max Usage** to around **3–5%**, depending on drive capacity and available space. Five percent is a suggested allocation, not a performance tweak.
6. If old restore points are using too much space and you accept losing that restore history, click **Delete**. This deletes all existing restore points for that drive.
7. Click Apply/OK, return to System Protection, and click **Create**.
8. Name it `Before Windows Optimization` and create it.

If you do not want to delete your existing restore history, skip step 6 and simply create a new restore point. Do not delete restore points after creating the one you intend to keep.

## 4. Clear Windows temporary files

Close open applications first. Press `Win + R`, enter each location separately, and press Enter.

### A. User temporary folder

```text
%temp%
```

- Press `Ctrl + A` and delete the selected contents.
- If Windows says a file is in use or cannot be deleted, choose **Skip**. Do not force-delete it.

### B. Windows temporary folder

```text
%windir%\Temp
```

- Approve the permission prompt if shown.
- Select the contents and delete what Windows allows.
- Skip anything in use.

### C. Local application temporary folder

```text
%localappdata%\Temp
```

This is usually the same user-local Temp directory reached through `%temp%`; check it if you want to verify the folder. You do not need to clean the same directory repeatedly.

### D. Prefetch folder — optional, not routine

```text
%windir%\Prefetch
```

Prefetch is a Windows-managed launch cache. Deleting it does **not** unlock extra CPU performance and can make some launches slower until Windows rebuilds its data. It is not a recommended monthly optimization. Leave it alone unless troubleshooting a specific cache issue or following a controlled diagnostic procedure.

### E. Empty Recycle Bin

Check the Recycle Bin first. Empty it only after confirming that you do not need anything inside it.

## 5. Run Disk Cleanup and Storage Sense

### A. Disk Cleanup

1. Press `Win + R` and run:

```text
cleanmgr
```

2. Select `C:`.
3. Click **Clean up system files** and select `C:` again.
4. Review the available categories. Common candidates include temporary files, thumbnails, DirectX Shader Cache, Delivery Optimization Files, update cleanup and error-reporting files.
5. **Review before selecting:** Downloads, Recycle Bin, Previous Windows installation(s), Windows ESD installation files and device-driver packages can remove data or rollback options you may need.
6. Click OK and confirm deletion.

Repeat for other drives only where useful.

### B. Storage Sense

- Windows 11: `Settings > System > Storage > Storage Sense`.
- Windows 10: `Settings > System > Storage`.

Enable Storage Sense if you want automatic cleanup. Configure it so the Downloads folder is not automatically deleted unless that is genuinely what you want. Run it manually after reviewing the categories.

### C. Windows.old and Windows 10 Upgrade files

If `C:\Windows.old` or a Windows upgrade folder exists, remove it through **Settings > System > Storage > Temporary files** or Disk Cleanup, not by force-deleting protected folders. Do this only after you are certain the new Windows installation works and you no longer need the rollback option. Removing the previous installation can eliminate the ability to return to the old Windows version.

### D. SoftwareDistribution\Download update cache

Folder:

```text
%windir%\SoftwareDistribution\Download
```

Prefer Windows Update Cleanup or Storage settings. Do not delete this folder while updates are downloading/installing. If Windows Update has a stuck/corrupt download cache, use this controlled reset from an Administrator Command Prompt:

```cmd
net stop wuauserv
net stop bits
```

Then delete the **contents only** of `%windir%\SoftwareDistribution\Download` in File Explorer. Restart the services:

```cmd
net start bits
net start wuauserv
```

Restart Windows and check for updates. This is a troubleshooting reset, not a routine FPS tweak.

## 6. Optimize your drives and check for errors

### A. Optimize Drives

Press `Win + R` and run:

```text
dfrgui
```

- **HDD:** Analyze, then Optimize/Defragment if Windows recommends it.
- **SSD:** Use Windows **Optimize**. Windows performs SSD maintenance such as TRIM; this is not the same as traditional HDD defragmentation.
- Leave scheduled optimization enabled unless you have a specific reason to change it.

Do not run third-party forced defragmentation on an SSD. Windows automatically manages drive optimization on normal installations.

### B. File-system error scan

For a basic online scan, open Terminal/Command Prompt as Administrator and run:

```cmd
chkdsk C: /scan
```

Use the drive letter that you want to check. Do not casually run `/f` or `/r` on every drive; those options can take longer and may require a reboot or extended disk activity. Back up important files first if a drive reports errors or is showing signs of failure.

---

# PART C — APP, BROWSER, STORE AND GPU CACHE

## 7. Clear File Explorer history

1. Open File Explorer.
2. Open **Folder Options** (Windows 11: three-dot menu > Options; Windows 10: View > Options).
3. On the General tab, find the Privacy section.
4. Click **Clear** to remove File Explorer history.
5. The **View** tab also contains the **Show hidden files, folders and drives** option. Enable it only when you need to inspect a hidden cache folder; you can turn it off again afterward.

## 8. Reset Microsoft Store cache

Close Microsoft Store, then press `Win + R` and run:

```text
wsreset.exe
```

A blank command window may appear briefly. The Store should reopen when the reset completes. Use this when the Store is slow, stuck or having download/cache problems; it is not a general FPS boost.

## 9. Clear browser cache without deleting saved passwords

Clear the cache of the browser(s) you actually use. Select **Cached images and files**. Cookies/site data may sign you out, while password and autofill deletion can remove stored credentials. Do not select those categories unless you intend to clear them and have a safe way to sign back in.

- **Chrome:** `Settings > Privacy and security > Delete browsing data`.
- **Edge:** `Settings > Privacy, search, and services > Clear browsing data > Choose what to clear`.
- **Firefox:** `Settings > Privacy & Security > Cookies and Site Data > Clear Data`; history cleanup is a separate option.

A browser may load a site slightly slower the first time after its cache is cleared because it needs to download assets again. Clearing cache does not increase your ISP's bandwidth.

## 10. Clear Spotify cache (only if large or problematic)

Close Spotify completely, including its tray/background process. For the regular desktop installer, inspect:

```text
%localappdata%\Spotify
```

The cache may be in a `Storage` or `Data` folder depending on the app version. Delete only the cache contents you have identified, not the entire Spotify profile. Microsoft Store builds may use a package-specific folder under `%localappdata%\Packages`. Prefer Spotify's own Storage/Clear Cache setting when available. Do not remove offline downloads unless you are happy to download them again.

## 11. Reset graphics shader cache (troubleshooting only)

Shader caches help games avoid recompiling frequently used shaders. Clearing them does not normally increase FPS; it can help if a cache is corrupted or using unusual disk space. The first launch after clearing may stutter while the cache is rebuilt.

### NVIDIA

Use the NVIDIA App if it provides a shader-cache reset/management option. For manual cleanup, NVIDIA's current support instructions identify cache locations under `%localappdata%\NVIDIA` and `%localappdata%\NVIDIA Corporation`, including `DXCache`, `GLCache` and `NV_Cache` depending on driver/app version. Close games first and delete only cache contents, not driver folders. Follow NVIDIA's current instructions for the installed driver version.

### AMD Radeon

In AMD Software: Adrenalin Edition, open Settings/Graphics/Advanced and use **Reset Shader Cache** if present. Menu names vary by version.

After resetting a graphics cache, launch the games you normally play and allow shader compilation to complete. Expect possible temporary stutter during rebuild.

---

# PART D — WINDOWS SETTINGS AND BACKGROUND LOAD

## 12. Startup apps and installed applications

### Startup apps

1. Press `Ctrl + Shift + Esc`.
2. Open **Startup apps** (Windows 10 may show a Startup tab).
3. Sort by Startup impact.
4. Disable nonessential apps you recognize and do not need immediately after login: launchers, chat clients, music apps, update helpers and utilities you open manually.
5. Keep security tools, touchpad/hotkey components, audio utilities, backup/sync tools you rely on, and unknown items until you identify them.

### Uninstall software you do not use

- Windows 11: `Settings > Apps > Installed apps`.
- Windows 10: `Settings > Apps > Apps & features`.

Remove known unused applications, old launchers, duplicate utilities, trialware and OEM software you do not need. Do not remove Microsoft Edge WebView2, runtimes, chipset utilities or driver components just because their names look unfamiliar; other applications may depend on them.

## 13. MSConfig boot options — timeout, No GUI Boot and processor count

Press `Win + R`, run:

```text
msconfig
```

Open the **Boot** tab and select your Windows installation.

- **Timeout:** The video reduces a 15–30 second boot-menu timeout to **5 seconds** (or 3 seconds). This only shortens the wait on the operating-system selection menu when that menu is shown; it does not make Windows itself load drivers or the desktop faster. Five seconds is a sensible value if you dual-boot. If you never see a boot menu, the practical difference may be negligible.
- **No GUI boot:** This hides the normal Windows boot animation. You can enable it if you prefer a plain boot screen, but treat it as a cosmetic change, not a meaningful performance boost.
- **Advanced options > Number of processors:** The video suggests checking this and choosing the highest number. **Do not do that as a performance tweak. Leave the box unchecked.** Windows uses the available processors by default; this option is useful for limiting processor count during troubleshooting, not unlocking hidden cores. Checking it can impose a limit rather than improve performance.
- **Maximum memory:** Leave unchecked. It is a diagnostic limit, not a way to unlock RAM.
- Do not enable Safe boot, debugging or other diagnostic startup modes unless you are troubleshooting.

Click Apply/OK when finished. Do not use **Make all boot settings permanent** as part of routine optimization. Restart only after completing the rest of your selected changes.

## 14. Optional Windows UI and multitasking changes

These settings mostly reduce visual clutter and small background activity. They are not major FPS boosters on modern hardware.

- **Notifications:** `Settings > System > Notifications`. Disable app notifications you do not need, but keep important security and system alerts.
- **Focus Assist / Do not disturb:** Configure it for gaming or full-screen work if notifications interrupt you. The Windows 10 name and Windows 11 menus differ.
- **Animations and transparency:** `Settings > Accessibility > Visual effects` (Windows 11) or `Settings > Ease of Access` / Personalization (Windows 10). Turn off Animation effects and Transparency effects if you prefer responsiveness over visuals.
- **Background apps:** Restrict background permission for individual apps that do not need it. Newer Windows builds may manage this per-app under Installed apps > Advanced options.
- **Offline maps:** Remove downloaded offline maps and disable automatic map updates if you never use them.
- **Virtual desktops / Snap:** Keep the features you use. Restricting Alt+Tab to the current desktop or turning off Snap is a workflow preference, not a meaningful general performance tweak.
- **Shared experiences / nearby sharing:** Turn off only if unused. Names and availability vary by Windows build.
- **Tablet mode (older Windows 10 2-in-1s):** Set the device to use the desktop experience on sign-in if that suits your workflow. This is not a performance tweak and is not a normal Windows 11 desktop setting.
- **Remote Desktop / Remote Assistance:** Turn off remote access only if you do not use it. Keep it enabled if you rely on RDP, remote support, a work PC or a hosted server workflow.
- **Wallpaper:** A solid-color background can reduce a tiny amount of visual workload compared with animated/live wallpaper. On a normal PC, this is a cosmetic/minor change, not a major performance upgrade.

## 15. Privacy and permissions

Review `Settings > Privacy & security` (Windows 11) or `Settings > Privacy` (Windows 10).

- Disable optional ad personalization, tailored experiences, optional diagnostic-data sharing, online speech recognition and typing/inking personalization if you do not use them. Where Windows offers a Required versus Optional diagnostic-data choice, choose the lower/required-only option if that meets your needs.
- Clear diagnostic data or location history if desired.
- Review app access to Location, Camera, Microphone, Contacts and Notifications. Deny permissions to apps that do not need them, but do not disable camera/microphone access globally if you use video calls, voice chat, scanning or recording.
- Keep Windows Security, Microsoft Defender, Firewall, UAC and security updates enabled.

Privacy changes may reduce background data exchange, but do not expect a large FPS increase from telemetry switches alone.

## 16. Gaming features

- **Game Mode:** `Settings > Gaming > Game Mode` — enable it and compare game performance.
- **Xbox Game Bar / Captures:** Disable background capture, automatic recording and overlays only if you do not use them. Keep them if you rely on screenshots, clips, controller overlays or Xbox features. If you keep recording enabled, choose a shorter recording length, 30 FPS and Standard quality only if those settings meet your needs.
- **TruePlay (legacy Windows 10 builds):** Leave it alone unless the option is present and you understand whether your Xbox gaming workflow uses it. It is not a relevant universal Windows 11 optimization.
- **Game DVR:** Turn off background recording if unused. Do not disable services that your Xbox/Game Pass/controller workflow requires.
- **Graphics preference:** `Settings > System > Display > Graphics`. Add a game and choose **High performance** when a laptop has both integrated and discrete GPUs.
- **Optimizations for windowed games (Windows 11):** Test the setting under `Settings > System > Display > Graphics > Default settings` for compatible DirectX 10/11 windowed or borderless games.
- **HAGS:** If supported, test Hardware-accelerated GPU scheduling on and off. Keep whichever produces better repeatable frame times on your hardware.

---

# PART E — VISUAL EFFECTS, MEMORY, AUDIO AND POWER

## 17. Advanced visual effects

1. Press `Win + R`.
2. Run:

```text
sysdm.cpl
```

3. Open **Advanced > Performance > Settings**.
4. Choose **Adjust for best performance**, or choose Custom and disable the effects you do not care about.
5. Consider re-enabling **Smooth edges of screen fonts** and **Show thumbnails instead of icons** for usability.
6. Click Apply.

Under **Advanced > Processor scheduling**, leave the default unless you have a specific workload reason. **Programs** is the usual desktop choice; switching to Background services is not a universal gaming tweak.

## 18. Virtual memory / page file

The page file is not simply unused RAM. It supports committed memory and crash-dump behavior. Do **not** remove all paging files or set the page file to zero to “free RAM.” Do not blindly set it to 1.5× installed RAM.

Recommended baseline:

1. Run `sysdm.cpl`.
2. `Advanced > Performance > Settings > Advanced > Virtual memory > Change`.
3. Keep **Automatically manage paging file size for all drives** enabled.
4. If you have intentionally configured a manual page file and need to correct it, use a system-managed page file on a fast internal SSD where possible, then restart.

The old video method of removing every paging file and recreating one on the SSD is a manual alternative, not a necessary optimization. Windows usually manages this correctly by itself.

## 19. Audio settings

1. Right-click the speaker icon and open Sound settings / More sound settings.
2. Select the active playback device and open Properties.
3. If you hear crackling, stutter or latency issues, test disabling audio enhancements/spatial effects.
4. Choose a supported default format appropriate for your device and content. **16-bit, 44.1 kHz is not a universal performance setting**; 24-bit, 48 kHz may be a better fit for many gaming/headset configurations.
5. Test audio after each change. Restore enhancements if sound becomes worse or a device feature stops working.

## 20. Power plan and CPU power management

- Desktop, plugged in: test **Balanced** versus **High performance**. Windows 11 may expose `Settings > System > Power & battery > Power mode > Best performance`.
- Laptop: use Balanced for normal use and Best performance/High performance while plugged in and gaming. Expect more heat, fan noise and battery drain.
- Do not blindly set the processor's minimum and maximum states to 100%. This can increase idle power, heat and fan noise without increasing actual game performance. Keep normal dynamic power management unless a measured issue requires a specific change.
- **Hard disk > Turn off hard disk after = 0 / Never:** only consider this for a desktop HDD if sleep/spin-down behavior is causing a real issue. It is not a universal SSD tweak.
- Ultimate Performance may exist on some Windows editions. Compare it with Balanced/High performance instead of assuming it is faster.

---

# PART F — SERVICES AND DEBLOATING

## 21. Review services carefully

Open `services.msc`. Do not disable services in bulk. The list below explains the common video suggestions:

| Service / feature | Recommended treatment | Reason |
|---|---|---|
| SysMain (Superfetch) | Keep default unless you can reproduce a disk-usage problem | It preloads data to improve app launch; disabling it can hurt some systems, especially HDD systems |
| Connected User Experiences and Telemetry (DiagTrack) | Adjust privacy settings first; do not expect a large RAM/FPS gain from disabling the service | Reported “hundreds of MB always used” claims are not a universal Windows baseline |
| Windows Search | Keep unless indexing is demonstrably causing persistent disk/CPU load and you accept slower searches | Indexing speeds up search results |
| Delivery Optimization | Turn off peer-to-peer downloads or cap upload/download rates in Settings if desired | Controls update sharing; it is not a hidden multi-GB RAM leak on every PC |
| Diagnostic Policy Service | Keep default | Supports Windows troubleshooting/diagnostics |
| Fax | Disable only if you never use fax functionality | Low impact; optional feature |
| Downloaded Maps Manager | Disable/limit only if offline maps are unused | Feature-specific, usually a small saving |
| Windows Image Acquisition (WIA) | Keep if you use scanners/cameras | Required by some imaging devices |
| Hyper-V / virtualization services | Keep if using Hyper-V, WSL2, Docker, emulators or VMs; otherwise manage the optional Windows feature deliberately | Disabling virtualization can break development and virtualization workflows |
| Memory Compression | Keep Windows-managed memory compression enabled | It is a normal memory-management feature, not a RAM leak to disable globally |
| IP Helper | Keep unless you know a specific dependency is unused | Used by IPv6 transition and other networking features |
| NDU (Network Data Usage Monitoring) | Leave default | Changing it to Start=4 is not an established universal “RAM leak fix” |
| Windows Update / BITS | Keep enabled | Needed for updates and background transfer tasks |

### How to review third-party services safely

1. Run `msconfig`.
2. Open **Services**.
3. Check **Hide all Microsoft services**.
4. Review the remaining third-party services by publisher and application.
5. Disable only a service you recognize and do not need. Change a small group at a time and note what you changed.

Do not select **Disable all**. Keep antivirus, VPN, backup, audio, touchpad, GPU, chipset, peripheral and hardware-monitoring services that you rely on. Do not disable Windows Memory Compression through PowerShell or registry edits as a generic “free RAM” tweak; Windows manages it according to memory pressure.

## 22. Debloating and cleanup utilities

- Start with `Settings > Apps > Installed apps` and uninstall programs you recognize and do not use.
- Disable unnecessary startup entries before removing system components.
- Avoid old one-click scripts such as legacy Windows10Debloater forks without checking maintenance status, current Windows compatibility and every proposed change.
- If using a power-user utility such as [Chris Titus Tech WinUtil](https://github.com/ChrisTitusTech/winutil), inspect the current options and preset contents, create a restore point, avoid aggressive presets, and do not run commands you do not understand. Its existence does not make every tweak appropriate for every system.
- CCleaner is not required for routine cleanup. Built-in Disk Cleanup and Storage Sense are sufficient for most users. Avoid registry-cleaning features and do not leave extra cleaners running in the background.

---

# PART G — NETWORK AND INTERNET OPTIMIZATION

## 23. Update and inspect the active network adapter

1. Press `Win + R`, run `ncpa.cpl`.
2. Identify the adapter currently in use: Ethernet or Wi-Fi.
3. Right-click it > Status/Properties, or open Device Manager to identify the exact adapter model.
4. Install the correct driver from the adapter or PC manufacturer's official site.
5. If you use Ethernet and experience link drops, packet loss or latency variation, open adapter Properties > Configure > Advanced and inspect Energy Efficient Ethernet / Green Ethernet / power-saving settings. Test disabling one setting at a time.
6. In the adapter's Power Management tab, test clearing **Allow the computer to turn off this device to save power** only if you experience disconnects or wake issues. On laptops, this can increase battery use.
7. Do not disable every power-saving feature by default, especially on Wi-Fi laptops.

## 24. Flush DNS and reset networking only when appropriate

Open Command Prompt as Administrator.

### DNS cache refresh

```cmd
ipconfig /flushdns
```

This clears the local DNS resolver cache. It can help with stale or broken name resolution; it does not increase bandwidth or reduce the ping to an already-connected game server.

### DHCP renewal / Winsock reset — troubleshooting

```cmd
ipconfig /release
ipconfig /renew
netsh winsock reset
```

`/release` temporarily disconnects the network until a valid address is renewed. Use these commands for an actual connectivity problem, not as a daily gaming ritual. Restart after Winsock reset.

Other commands sometimes found in guides:

```cmd
ipconfig /registerdns
```

Use this when troubleshooting DNS registration, not as a routine speed boost.

## 25. DNS server selection

You can compare your ISP-provided DNS with public resolvers:

**Google DNS**

```text
8.8.8.8
8.8.4.4
```

**Cloudflare DNS**

```text
1.1.1.1
1.0.0.1
```

Set DNS under `ncpa.cpl > active adapter > Properties > Internet Protocol Version 4 (TCP/IPv4) > Properties`. You can also configure IPv6 DNS if your network and provider support it. Do not disable IPv6 just because an optimization video suggests it.

Choose based on repeatable lookup reliability and latency from your location. DNS mainly affects how quickly a domain name is resolved; it does not change the route or ping once a game has connected to its server. A DNS benchmarking utility can compare resolvers, but the result is location- and ISP-dependent. Namebench is an old utility; do not treat an old benchmark result as permanently accurate.

## 26. Delivery Optimization bandwidth caps

Windows can share update files with other PCs. If you do not want peer-to-peer sharing:

- Windows 11: `Settings > Windows Update > Advanced options > Delivery Optimization`.
- Windows 10: `Settings > Update & Security > Delivery Optimization`.

Turn **Allow downloads from other PCs** off if you do not use it. Alternatively, open Advanced options and cap background download, foreground download and upload bandwidth. A **5% cap** is a reasonable conservative starting point if you want to limit update traffic while using the PC. It is a cap on Delivery Optimization traffic, not a magical latency improvement.

Schedule active hours so Windows does not unexpectedly restart during your normal usage. Do not disable Windows Update completely.

## 27. QoS Packet Scheduler and the “20% reserved bandwidth” claim

On Windows Pro/Enterprise editions, `gpedit.msc` may expose:

`Computer Configuration > Administrative Templates > Network > QoS Packet Scheduler > Limit reservable bandwidth`

Some videos recommend enabling it and setting the value to `0%`. This policy controls the amount of connection bandwidth the system can reserve for QoS-aware applications; it does **not** mean Windows permanently holds 20% of every internet connection idle and unusable. Setting it to 0 is not a guaranteed speed boost and is normally unnecessary. Windows Home may not include the Group Policy Editor.

## 28. TCP Optimizer and advanced TCP settings

TCP Optimizer can expose many Windows network parameters. It is not a universal “faster ping” button. Before using it, save/export the current settings, select the actual internet connection speed rather than blindly maxing the slider, change only a small number of values, and keep a way to restore defaults.

| TCP Optimizer option mentioned in the video | Video's suggested value | Safer general recommendation |
|---|---|---|
| MTU | 1500 | 1500 is common for standard Ethernet, but PPPoE, VPNs and some ISP paths require a smaller MTU. Do not force 1500 blindly. |
| TCP Window Auto-Tuning | Normal | Keep Windows default/normal. |
| Windows Scaling Heuristics | Disabled | Do not change without a measured reason. |
| Congestion Control Provider | CTCP | Do not force CTCP universally; Windows defaults and availability differ by version. |
| RSS | Enabled | Keep enabled when supported. |
| RSC | Enabled | Usually useful for throughput; test only if a low-latency workload shows a measurable problem. |
| TTL | 64 | Leave default; changing TTL does not make the connection faster. |
| ECN | Disabled | Leave driver/OS default unless your network requires a specific setting. |
| Checksum Offloading | Disabled | Usually keep enabled; hardware offload can reduce CPU work. |
| TCP Chimney Offload | Disabled | Do not try to enable this legacy/deprecated feature. |
| Large Send Offload | Disabled | Usually keep enabled; disable only as a targeted driver troubleshooting test. |
| TCP timestamps | Disabled | Leave default unless troubleshooting a specific network path. |
| Max SYN retransmissions / Initial RTO / Minimum RTO | `2` / `2000` / `300` | Leave Windows defaults; arbitrary RTO changes can reduce reliability. |
| Non-SACK RTT resiliency | Disabled | Leave default. |
| QoS Non-Best-Effort Limit | `0` | Not a general gaming-speed tweak; see the QoS section above. |
| QoS “Do not use NLA” | “Optimal” | Leave default unless you know the exact policy and its purpose. |
| Local / Host / DNS / NetBT priority | `4 / 5 / 6 / 7` | Leave default. |
| NetworkThrottlingIndex / SystemResponsiveness | `FFFFFFFF` / `0` | Advanced registry values; do not assume universal benefit. See Registry section. |
| TCPNoDelay / TcpAckFrequency / TcpDelAckTicks | `1 / 1 / 0` (commonly circulated) | Do not apply as a global preset; behavior is workload and adapter-specific. |
| LargeSystemCache / Size | `0 / 1` | Leave defaults for a desktop/gaming PC. |
| MaxUserPort / TcpTimedWaitDelay | `65534 / 30` | Leave defaults unless diagnosing a server/socket-exhaustion workload. |

Microsoft documents that network offloads such as checksum offload and Large Send Offload can reduce host CPU work. Disabling them all because a video says so can reduce throughput or increase CPU overhead. TCP Chimney Offload is a legacy feature and should not be enabled.

## 29. Bufferbloat and router-side latency

If ping rises sharply while someone is downloading, streaming or uploading on the same connection, the issue may be bufferbloat or router queue management rather than Windows. Test latency while the connection is under load. Where supported, configure router SQM/QoS (for example, CAKE or fq_codel) using a carefully measured rate below the real line rate. This often matters more for loaded latency than changing Windows registry values.

---

# PART H — REGISTRY TWEAKS: ADVANCED, OPTIONAL, REVERSIBLE

**Create a restore point and export the exact key before editing.** Open Registry Editor with `regedit`. If you do not know how to restore an exported key, do not edit it.

## 30. Multimedia SystemProfile values

Path:

```text
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile
```

Popular gaming videos suggest:

- `NetworkThrottlingIndex` = `FFFFFFFF` (hexadecimal)
- `SystemResponsiveness` = `0`

These are not magic FPS switches. Microsoft's MMCSS documentation says `SystemResponsiveness` values below 10 are clamped to 20, so setting it to `0` does not mean “reserve zero for background tasks.” MMCSS is part of multimedia scheduling; changing it without understanding the workload can affect media/audio scheduling.

## 31. Games task scheduling values

Path:

```text
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile\Tasks\Games
```

Popular suggestions:

- `GPU Priority` = `8`
- `Priority` = `6`
- `Scheduling Category` = `High`

Treat these as advanced experiments, not mandatory defaults. Export the key first and compare actual frame-time results. Do not change unrelated task values.

## 32. Other registry tweaks that circulate online

| Tweak | Common video edit | Recommendation |
|---|---|---|
| Hardware-Accelerated GPU Scheduling | `HwSchMode=2` under `GraphicsDrivers` | Prefer the Windows Graphics Settings interface and test on/off where supported. Do not force an unsupported mode through the registry. |
| NDU Network Data Usage | `Start=4` under `Services\Ndu` | Leave default; disabling it is not a proven universal RAM-leak fix. |
| Clear page file at shutdown | `ClearPageFileAtShutdown=1` | Not a speed tweak; can increase shutdown time and disk activity. Leave default unless required by a security policy. |
| Game DVR | `GameDVR_Enabled=0` and policy values | Prefer Windows Gaming/Captures settings. Avoid conflicting per-user and policy registry edits. |
| `Win32PrioritySeparation` | Various values | Do not copy values from another PC; Windows scheduler behavior is version and workload dependent. |
| `NetworkThrottlingIndex` | `FFFFFFFF` | Optional experiment only; benchmark and restore if there is no improvement. |
| `SystemResponsiveness` | `0` | Values below 10 are clamped to 20 in MMCSS; not a literal zero reservation. |

Do not make a single registry file that applies all values automatically. Preserve the originals and change one setting at a time.

---

# PART I — CPU, GPU, BIOS AND INPUT OPTIMIZATION

## 33. Core parking and CPU frequency scaling

Tools such as QuickCPU and ParkControl expose core parking and frequency-scaling policies. Core parking is a normal Windows power-management feature, not a pool of disabled cores that must be “unlocked.”

- Keep Windows defaults first.
- If testing QuickCPU/ParkControl, create a restore point and note the original profile.
- Compare Balanced/High Performance and the tool's settings using the same game and benchmark.
- Watch CPU temperature, power draw, fan noise and battery life.
- Do not assume every core must be unparked or frequency scaling disabled.

## 34. Process Lasso, CPU priority and affinity

Process Lasso can help diagnose a background process that repeatedly steals CPU time. Start with monitoring and ProBalance rather than assigning aggressive priorities to every game.

- Avoid **Realtime** priority.
- Do not copy CPU affinity masks from another processor.
- Do not disable SMT/Hyper-Threading globally because one video recommends a core-count threshold.
- Test a per-game affinity only when a specific title has a reproducible scheduling problem, then revert if frame pacing worsens.

## 35. Timer Resolution utilities

Some utilities request a shorter Windows timer interval (for example, a “Maximum” setting). This can change timer behavior for applications that request it, but it is not a guaranteed FPS or input-lag improvement.

Only test a reputable, well-understood tool for a measured latency issue. Run it for the game session, then restore **Default** afterward. Avoid unknown timer utilities and permanent background operation.

## 36. GPU control-panel settings

Apply settings per game rather than forcing aggressive global overrides.

### NVIDIA

- Select the correct GPU for applications on hybrid systems.
- Use the game's NVIDIA Reflex option when supported; do not blindly stack every low-latency override.
- Test Power Management Mode per game if clocks are downclocking unexpectedly; maximum performance globally can increase idle power and heat.
- Leave shader cache enabled/driver-managed unless diagnosing a corrupted cache.
- Set frame caps, V-Sync, G-Sync/VRR and low-latency options according to the monitor and game. There is no universal best combination.
- GPU overclocking/undervolting is an optional separate tuning exercise, not part of the safe baseline. Change clocks/voltage gradually with a reputable vendor utility, monitor temperatures and power, and stress-test. Revert immediately if artifacts, driver resets or crashes appear.

### AMD Radeon

- Keep Shader Cache enabled/AMD Optimized for normal use; reset it only for cache troubleshooting.
- Keep Surface Format Optimization at its recommended/default setting unless a game requires a specific override.
- Avoid blindly forcing texture filtering, tessellation or synchronization settings globally. Use game-specific profiles and compare image quality and frame times.
- Test Radeon Anti-Lag or other supported latency features in the games where they apply.

### Intel graphics

Use Intel Graphics Software/Arc Control settings available for the exact GPU. Do not apply NVIDIA or AMD registry instructions to Intel hardware.

## 37. Hardware acceleration in background applications

Some optimization videos recommend turning off hardware acceleration in Chrome, Spotify and Discord. This is conditional:

- If a background app causes GPU contention, display glitches or stutter while gaming, test turning its hardware acceleration off.
- If the CPU then becomes busier or video playback becomes worse, turn it back on.
- Do not disable hardware acceleration in every application by default. On many systems it reduces CPU workload.

## 38. BIOS / UEFI checks for POWER users

BIOS changes can matter, but incorrect settings can prevent booting. Photograph or record every original setting before changing anything.

- **BIOS update:** Use the exact motherboard/laptop model's official firmware and instructions. Update only for a relevant fix, compatibility need or security reason. Keep stable power during the update; never interrupt flashing.
- **XMP / EXPO / DOCP:** Enable the correct memory profile if the RAM, CPU and motherboard support it. Then validate with a memory test and real workloads. Unstable memory overclocking can cause crashes and file corruption.
- **Resizable BAR + Above 4G Decoding:** Enable only if the CPU, motherboard, BIOS and GPU support it. Follow the board/GPU vendor's instructions and benchmark the games you play.
- **SMT / Hyper-Threading:** Leave enabled by default. Test a game-specific exception only when evidence shows it helps.
- **CPU C-states and power states:** Do not disable globally for gaming based on a generic video. They affect idle power, heat and platform behavior.
- **TPM and Secure Boot:** Keep enabled on supported Windows 11 systems. Do not turn off security features as a generic performance tweak.

## 39. Validate hardware before chasing software tweaks

Use reputable diagnostic tools from their official publishers to inspect:

- CPU model, clocks and RAM configuration (CPU-Z / HWiNFO).
- GPU model, clocks, temperature and utilization (GPU-Z / HWiNFO).
- CPU, GPU and memory stability (OCCT or another established stress-test tool).
- RAM errors (Windows Memory Diagnostic or MemTest86).
- Drive health and SMART data (manufacturer utility or reputable drive-health tool).

Monitor temperatures and clock behavior under load. Thermal throttling, a power-limited GPU, single-channel/incorrectly configured RAM, an overheating laptop or a failing drive can matter much more than registry edits.

## 40. Monitor, mouse and controller setup

- Set the monitor to its maximum supported refresh rate in `Settings > System > Display > Advanced display`.
- Use the correct cable/port for the refresh rate and resolution.
- Set monitor overdrive to a moderate setting; the maximum mode may introduce overshoot/ghosting.
- Configure FreeSync/G-SYNC/VRR according to the monitor and GPU support.
- Disable **Enhance pointer precision** only if you prefer raw/direct mouse input; set DPI and in-game sensitivity intentionally.
- Use stable, vendor-supported mouse/controller polling rates. Avoid unsigned USB polling-rate patches or controller overclocking tools that can destabilize USB devices.

---

# PART J — FINAL HEALTH CHECK, RESTART AND VALIDATION

## 41. Repair Windows component and system files, if they need to be, just run, Win will decide...

Open Terminal / Command Prompt as Administrator. Run DISM first, then SFC:

```cmd
DISM /Online /Cleanup-Image /RestoreHealth
sfc /scannow
```

Optional DISM health checks:

```cmd
DISM /Online /Cleanup-Image /CheckHealth
DISM /Online /Cleanup-Image /ScanHealth
```

These cmds repair/check Win components & protected system files. They are maintenance/troubleshooting tools, so let each command finish before starting the next one.

## 42. Malware scan

Open `Settings > Privacy & security > Windows Security > Virus & threat protection` and run a scan, its Win provided built in, You can also run:

```text
mrt
```

Microsoft's Malicious Software Removal Tool is not a replacement for Microsoft Defender. Keep real-time protection enabled.

## 43. Restart and compare

1. Save open work and restart Windows.
2. Confirm audio, Wi-Fi/Ethernet, Bluetooth, USB devices, camera, touchpad, printing and other hardware still work.
3. Check Task Manager for abnormal idle CPU, memory or disk usage.
4. Repeat the same system, game and network benchmarks from Section 1.
5. Compare average FPS, 1% lows, frame-time consistency, boot time, loaded ping, jitter, packet loss and disk space.
6. Revert any change that makes results worse or breaks a feature.
7. Create a second restore point such as `After Stable Optimization` once you are satisfied and the system has remained stable.

---

# QUICK MAINTENANCE CHECKLIST

| Frequency | Maintenance |
|---|---|
| After a major change | Restart, test hardware/features, compare benchmarks, create a restore point when stable |
| Monthly / as needed | Review startup apps, uninstall software you no longer use, check Storage Sense and free space |
| When troubleshooting | Clear the relevant cache, flush DNS, reset Store cache, inspect Event Viewer/Task Manager, run system checks |
| When a driver issue occurs | Use official driver rollback/update; use DDU only when a clean GPU driver removal is justified |
| When a game is updated or performance changes | Re-test game-specific GPU settings, frame cap, HAGS, overlays and background apps |
| Periodically | Check Windows Update/support status, GPU/chipset/network driver updates, drive health and temperatures |

## Tweaks NOT recommended as universal settings, you try to see how much of a difference it makes for you...

- Selecting the maximum processor count in `msconfig` (leave it unchecked; Windows already schedules available processors).
- Clearing Prefetch on a routine schedule.
- Setting the page file to zero or a fixed `1.5x RAM` without a workload reason.
- Forcing CPU minimum and maximum state to 100% on every PC.
- Disabling SysMain, Windows Search, Diagnostic Policy, NDU, memory compression or all Xbox services as a blanket RAM optimization.
- Disabling checksum offload, Large Send Offload, RSS or RSC as a universal network preset.
- Setting QoS reservable bandwidth to zero because Windows supposedly keeps 20% of the connection idle.
- Disabling IPv6, Defender, Firewall, UAC, Memory Integrity, VBS, Secure Boot or CPU mitigations just to chase a claimed FPS percentage.
- Applying random TCP registry values, timer-resolution tools, CPU affinities or “real-time” priorities globally.
- Using registry cleaners, unsigned drivers, cracked debloat scripts or experimental NVMe drivers.

**THESE SETTINGS WILL PRODUCE DIFFERENT RESULTS ON DIFFERENT DEVICES. APPLY CHANGES GRADUALLY, BENCHMARK THEM, KEEP WHAT WORKS, AND REVERT WHAT DOESN'T. :)**

---


### my technical references nd sources

- [Microsoft — Tips to improve PC performance in Windows](https://support.microsoft.com/en-us/windows/experience/performance-optimization/tips-to-improve-pc-performance-in-windows)
- [Microsoft — Defragment and optimize data drives](https://support.microsoft.com/en-us/windows/experience/storage-filemanagement/defragment-optimize-your-data-drives-in-windows)
- [Microsoft — Multimedia Class Scheduler Service](https://learn.microsoft.com/en-us/windows/win32/procthread/multimedia-class-scheduler-service)
- [Microsoft — Network adapter performance](https://learn.microsoft.com/en-us/windows-hardware/drivers/network/performance-in-network-adapters)
- [Microsoft — QoS Packet Scheduler policy](https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-admx-qos)
- [Microsoft — Memory Integrity / VBS](https://learn.microsoft.com/en-us/windows/security/threat-protection/device-guard/enable-virtualization-based-protection-of-code-integrity)
- [Microsoft — Windows 10 end of support](https://support.microsoft.com/en-us/windows/deployment/updates-lifecycle/windows-10-support-has-ended-on-october-14-2025)
- [Microsoft — Windows 10 Consumer ESU](https://www.microsoft.com/en-in/windows/extended-security-updates)
- [NVIDIA — Deleting NVIDIA Shader Cache files](https://nvidia.custhelp.com/app/answers/detail/a_id/5735/kw/menu/related/1)
- [AMD — Customize graphics settings in Adrenalin](https://www.amd.com/en/resources/support-articles/faqs/dh3-012.html)
- [Chris Titus Tech WinUtil — project and configuration](https://github.com/ChrisTitusTech/winutil)
- [GRC — DNS Benchmark](https://www.grc.com/dns/Benchmark.htm)
