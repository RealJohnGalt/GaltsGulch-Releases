# Changelog

**v1.0b33**
* Bugfix release: revert zram.

**v1.0b32**
* Bump toolchain (llvm master built today).
* gpu: revert lower slot gpu undervolt. Some users have instabilities and then crashes because of this change. For anyone else, KonaBess exists.
* cpu frequency table: add back 576mhz for little cluster.
* cpu frequency table: add back 2016mhz, 1804mhz, and 1497mhz on prime.
* cpufreq schedutil: Port changes around prioritizing per-cluster efficient frequencies from msm-4.19 schedhorizon by xzr467706992.
* config: bring back zram + swap. There are a lot of scenarios where it helps (unless you're a derp guac user), and not like we're swapping like crazy in this configuration anyway.
* config: re enable slub cpu partial due to regressions.
* proc/cmdline: bring back safetynet hacks. Unfortunately it seems some apps care even with the system/core implementation.

**v1.0b31**
* Long-awaited fix for cam issue on derp and other roms. One user also had reported on YAAP.
* Revert zram.
* cpusets: restructure for jank, since cpu4 is our perf worker type core.
* cpuset/uclamp assist: expand/tune.

**v1.0b30**
* Bump toolchain (llvm master built today).
* Merge upstream linux kernel 4.14.239.
* mm: a few improvements for simple LMK.
* Move a few more workqueues to power efficient workqueues (we've had enabled since initial release).
* config: drop simple lmk timeout to 100ms. In testing, 200 ms is too high for a good UX.

**v1.0b29**
* Bump toolchain (llvm master built today).
* Revert two memory related changes.
* Add automatic compaction and enable zram.
* Ion reverts for graphical glitches introduced in b27.

**v1.0b28**
* treewide: remove OnePlus SSR additions and dependencies. Additionally fully kill crashdump mode. In initial OP import, it was thought these additions were necessary. Also decreases boot time.
* Merge new CAF tag LA.UM.9.1.r1-10600-SMxxx0.0 into kernel and submodules.
* treewide: move some modules to Ofast optimization level which show benefits in benchmarks. None have FP arithmetic so generally considered "safe."

**v1.0b27**
* Bump toolchain (llvm master built today).
* Merge upstream linux kernel 4.14.238.
* Merge upstream android-4.14-stable.
* Merge upstream f2fs-stable.
* Revert 1440p dsc slice change: On some trees (roms) lesser than yaap, this caused crashdumps on 1440p devices.
* Bring in and enable playstation dualsense support + force feedback.
* simple lmk: Attempt to improve UX with memory pressure and display state awareness, also support OP AOD.
* mm/vmpressure: kerneltoast change for simple lmk.
* sched: backports and performance optimizations.
* ion: some backports and enable pool auto refill.

**v1.0b26**
* Bump toolchain (llvm master built today).
* Fingerprint on Display: boost devfreq and prime core on press.
* cgroup: boost devfreq for a short time on top app change.

**v1.0b25**
* Bump toolchain (llvm master built today).
* Fix camera issue introduced for some users in b24.
* Some wakeup/suspend/idle fixes.
* lz4: backports.
* fqcodel: backports.
* optimization: selectively use Ofast in some spots that can measurably benefit.
* input/touchscreen: revert changes in b24.

**v1.0b24**
* Bump toolchain (llvm master built today).
* Fix a crashdump.
* input/touchscreen: increase sensitivity, decrease latency.
* block: enable and tune throttling.
* various other small optimizations.
* qcacld-3.0: kill remaining wakelock code.
* drivers/gpu/drm/msm: kill sde tracing entirely.

**v1.0b23**
* Bump toolchain (llvm master built today).
* Minor treewide fixes and cleanups.
* Kill schedutil rate limits entirely.
* Drop some debugging.
* panel/samsung oneplus: set 1440 dsc slice per packet to 4 (NOT IN OOS BRANCH).
  * Theoretically the max before we should see an increase in frame times. Since 1440 needs dsc compression, attempt to lessen the overall cost.
* Simple LMK: drop minfree to 128.
* drm/msm: some arter97 optimizations.

**v1.0b22**
* Merge treewide (submodules incl) LA.UM.9.1.r1-10500-SMxxx0.QSSI12.0.
* Merge upstream f2fs stable.
* Bump toolchain (llvm master built today).
* hid/nintendo: bring inline with upstream maintainer's git, enable force feedback support. Joycon double presses are a userspace issue and not present in all apps either.
* gulch config: switch to 12ms PELT half life (from 24ms):
  *  Since the per cluster rate limit tuning in schedutil, we no longer benefit from the longer decay. However the shorter ramp up does benefit us while profiling more interactive tasks.
* gulch config: revert to 80hz timer rate from 240hz for better power efficiency.
* Revert a hack no longer needed for a llvm master workaround.
* *OOS BRANCH ONLY* Revert all CAF merges to resolve cam issues and potentially more.

**v1.0b21**
* Merge upstream linux kernel 4.14.237.
* Bump toolchain (llvm master built today).
* Implement workaround for new llvm master toolchain.
* config: drop devfreq boost duration (like on new frame commits) from 64ms to 48ms. No regressions found in testing.
* arm64/vdso32: use ld.lld to link. Previously this was the only part of our tree still using ld.bfd.
* bpf jit: small performance optimization.
* cpuidle: revert max s2idle attempts. In reality the only driver that occasionally needed this was alarmtimer, and created a regression.
* block: disable io stats, enable write back caching (in testing, no regressions found).
* gpu/kgsl: schedule perf worker thread as rr rather than fifo (in testing, no regressions found).

**v1.0b20**
* Bump toolchain (llvm master built today).
* Introduce devfreq-cpufreq governor.
* Some minor backports.
* Introduce and merge LA.UM.9.1.r1-10200-SMxxx0.0 of forgotten data-kernel for rmnet extensions. These help network performance and efficiency considerably.

**v1.0b19**
* Merge upstream linux kernel 4.14.236.
* Fix CachedAppOptimizer access (permissions now inline with pissels).
* Makefile: optimize for cortex-a76 on clang:
  * On 50 scripted hackbench -l 10000 -T 8 runs, improved performance by 1.81%. Also results in more efficient output, with the uncompressed output binary size being reduced by 5.347%.
  * Since nothing latency or performance sensitive is placed on little cluster anyway, the small little cluster efficiency gains from optimizing for cortex-a55 don't matter.
* drm atomic: disable devfreq frame commit boosts on AOD: while obviously we're not pushing many frames on AOD, this is still unnecessary.
* f2fs: fixup f2fs interface renaming for compatibility with DSU and OTA, while still blocking userspace from triggering GC.
* char: switch to srandom, while retaining our previous change of using urandom for random fops if disabled (for proper testing).

**v1.0b18**
* Fix random reboots on shell (tty, from poad42).
* wireguard: bring inline with mainline linux kernel (new changes from 4 days ago). Nice benefit is reduced memory usage, can be large in some scenarios.
* wlan: switch all related submodules to LA.UM.9.1.r1-10200-SMxxx0.0. Should fix a relatively rare crashdump.

**v1.0b16**
* Merge upstream linux kernel 4.14.235.
* Merge latest CAF tag for our device of LA.UM.9.1.r1-10200-SMxxx0.0.
* Merge June ASB.
* Partially revert big cluster rate tuning. We don't need the lower up rate, only the longer hold. For some usecases, this decreased battery life, and increased thermal mitigation hits.

**v1.0b15**
* gpu: bring back lower slot undervolt. This was in old releases for awhile, and there was never a convincing crash or soft reset reported from this.
* treewide: resolve power regressions from b14.
* f2fs: merge upstream f2fs stable.
* Introduce support for affining perf kthreads and irq to specific big cores. Overall these changes improve jitter consistency.
  * treewide: move non-latency sensitive, but perf sensitive worker-type kthreads to cpu4. For power efficiency.
  * msm/kgsl: affine irq and worker kthread to cpu5.
  * fs/exec: affine hardwarecomposer to cpu6.
* block/loop: move kthread to prime due to low utilization but high latency sensitivity.
* wlan: disable CxPC aware RTPM. While we support basic PM, we don't support this.
* config: tune big cluster rate limits for better interactivity.
* idle: more s2idle fixes.
* adsprpc: revert unnecessary and problematic change. 

**v1.0b14**
* kgsl: make mem workqueue freezable. freezing during no interactivity may benefit power.
* media: use interruptible waits.
* fs: reduce cache pressure in attempt to better utilize ram.
* mm: wake kswapd sooner.
* config: bump vmstat interval to 60s. It's far too costly to run every 20s.
* wlan: merge LA.UM.9.14.r1-16700-LAHAINA.0 in wlan-related submodules.

**v1.0b13**
* arm64/dts: revert key latency change. Some users reported single keypresses reported as two.
* sched: two reverts with unintended consequences.
* sched: backports.
* treewide: some moves to interruptible waits.
* PM: tweaks for better race to idle performance.
* mm: backports and mark some perf critical kthreads for big cluster.
* rcu: backports and mark some perf critical kthreads for big cluster.
* config: enable rcu boost. This only occurs during preemption, in which scenarios we're latency sensitive.
* kswapd: affine to little cluster for efficiency.
* config: set little cluster rate limits same as prime for small power savings with no interactivity hit.
* *OOS* fixup some display mode memes breaking ambient display or harming FOD performance.

**v1.0b12**
* Merge upstream linux kernel 4.14.234.

**v1.0b11**
* arm64/vdso32: support integrated assembler and full clang build. Decreases output binary size considerably due to better linking decisions.
* msm geni serial: revert recent backports creating BT memes.
* arm64/dts: possibly decrease key latency.

**v1.0b10**
* toolchain bump (llvm master built today).
* device mapper: backport optimizations. These should theoretically have the largest impact in recovery and fastboot.
* power: update usb switching charge adapter + peripheral fix.
* display: revert panel on delay to 120ms from 10ms.
  * On some devices, panel would wake up before mode was set for FOD, resulting in poor FOD performance.

**v1.0b9**
* toolchain bump (llvm master built today).
* kgsl: increase events worker thread priority.
* kgsl: bring in dependencies for an earlier backport.
* mm: backports and optimizations.
* adsprpc: force QoS to silver cores for power savings. According to CAF, this "saves significant power."
* scsi/ufs: reduce potential for contention.
* net: some backports.
* block: various backports.
* schedutil: backports.
* debugging: bring in last kmsg support, additionally re enable bug for now.
* arm64/vdso: compat bringup, with LTO still enabled.
* uclamp assist: tweak top app for stabilizing jitter and reducing power consumption.
* net/qrtr: reduce likelihood of missing packets due to sleep (CAF backports).
* msm geni serial: various CAF backports.
* page alloc: ensure to boost devfreq if slow path is hit as well.
* qca-wifi-host-cmn: reduce annoying spam.
* arm64/lse atomics: backport cleanup.
* Revert some older async probe changes from google — we need more treewide fixes for these, and the full patchset caused boot issues for guac as well as suspend/resume issues for hdb as well.

**v1.0b8**
* Revisit s2idle.
* Merge Linux Kernel 4.14.233.

**v1.0b7**
* toolchain bump (llvm master built today).
* Now not only the kernel with the lowest jank/jitter, also the best efficiency: killed cpu input boost. This only harms jitter for our configuration, along with a major power impact. Especially good to kill given the per-cluster rate limit tuning (mentioned below).
* sched: many backports and placement improvements.
* sched: introduce rate limit tuning per cluster. Tune for performance and efficiency (synthetic tests have no regressions while more efficient frequencies are used overall).
* power: usb charging fix from sm8250.
* kcal: new sde implementation that actually works.
* many other backports/optimizations.
* wireguard: bring fully inline with mainline linux kernel (were 2 months behind).
* wlan-related submodules: merge LA.UM.9.14.r1-16300-LAHAINA.0 
* f2fs: rapid gc bringup.
* exfat: import, enable, and update with upstream.
* charging: fix regression.
* lib: new optimized routine backports.
* lse atomics: remove dynamic alternatives patching for lower overhead since we know we support lse atomics and enable unconditionally.
* fod: merge new changes for yaap and new rom builds. Moves a platform hack to kernel.
* wlan: force a power management mode for roms without the change in DT. 

**v1.0b6**
* misc: correct a few wakeup hangs, including one potential never hit.
* wlan: bump to lahaina tags to match yaap oss tags.
* toolchain: bump to latest llvm master (just built).
* correct cpu boost freqs.
* bump ddr bw boost for frame commits after more jitter testing, and 7pro testing.
* cpuidle: remove another s2idle leftover only causing harm.
* sched: many backports, add back part 2 for making up/down migrate margins non-configurable.
* bt: improve performance slightly.
* adrenotz: revert change that increases scaling on 90hz (at best does nothing, at worst increases jitter and power consumption).
* fuse: properly port a oneplus optimization + some cleaned up deps to our caf tree.
* mm: some backported optimizations.
* treewide: move some non performance sensitive workqueues to power efficient workqueues.
* scsi/ufs: performance and power improvement.
* kgsl: adapt a google commit for better nonblocking performance, maintaining high performance kthreads as such.
* kgsl: backport a locking improvement.
* affinity: move surfaceflinger to prime:
  * surfaceflinger and hwcomposer should not be contending with one another. In testing, surfaceflinger was more latency sensitive than hwcomposer. Additionally in various real world testing we always have the capacity necessary for this change. Test: lessens jitter spikes in high contention scenarios.

**v1.0b6a2**
* config: Expand game controller support.
* security: Initialize everything on the stack with a zero value for security.
* A few new kerneltoast optimizations:
  * mm: perform PID map reads on the little cpu cluster. They're not performance sensitive, but can be very costly.
  * sched: improve code optimization.

**v1.0b5**
* Merge latest ASB (2021-05-05).
* Fix additional wake issues (reverted s2idle leftovers).
* Restore iorap support, verified working on YAAP. Helps cold startup launch times. For more info: https://medium.com/androiddevelopers/improving-app-startup-with-i-o-prefetching-62fbdb9c9020
* Android/binder: some backports.
* Net/bbr2: some more backports.
* kernel/sched: uclamp schedtune compatibility cleanup.
* drm commit ddr boost: slightly bump after additional jitter and jank testing on 7Pro. Big thanks to zero too. Even on 7T this change helped jitter.
* Revert long forgotten gpu undervolt.
* Partially revert cpuidle tweak.
* some cleanup.
* sched/uclamp:
  * More backports.
  * Make backwards compatible with schedtune/cpusets.
  * Introduce uclamp assist.
  * Drop schedtune in favor of uclamp assist.
* Makefile: ThinLTO tweaks:
  * Enable fwhole-program-vtables with ThinLTO for better inlining decisions (0.00489045% binary size decrease).
  * Set import-instr-limit to 40:
  * Decreases output size by 10.0308%, and also where measurable performance changes stop occurring. Chromium found 10 was a good limit for performance/binary size, and AOSP found 5 was a good compromise. However we're a kernel and a bit different with more potential to benefit from aggressive inlining.

**v1.0b4**
* Merge new CAF tag LA.UM.9.1.r1-09600-SMxxx0.0 tree wide.
* Bring in klapse.
* Display: ensure ulps is always on, and enable partial update with singleroi.
* Revert s2idle: a few users still had wakeup issues.
* msm thermal simple: tweak battery temp range to make more conservative. Results in better warp charging throughput, and uses the battery less as a heatsink when active.
* Various optimizations and cleanups.
* timer: switch to 240hz for better frame cadence (previously 80hz).
* uclamp: switch to 7 uclamp buckets for improved placement, also needed for possible upcoming device tree changes.
* sched: revert pelt half life to 24ms due to longer decay assisting jitter.
* cpuidle: decrease deep idle latency reporting for more deep idle consideration.
* frequencies: big cluster: introduce lower frequencies.
* frequencies: prime: bump min freq to a less efficient 2227200 for better jitter, and also better utilization of big cluster low frequencies for better real world power usage.
* thermal: tweak battery temp range to make more conservative. Results in better warp charging throughput, and uses the battery less as a heatsink when active.
* thermal: update for new table. 
* toolchain bump (built today off current llvm master). 

**v1.0b3**
* Bring back unintentionally forgotten cpuset assist.
* sched: drop pelt half life to 10ms for interactivity and efficiency.
* cfq: some optimizations and backports.
* other small changes.
* toolchain bump (built today).

**v1.0b2**
* More s2idle fixes.
* sched: many backports, including full uclamp bringup.
* config: debloat.
* Merge upstream OP fixes.
* display: reduce panel on time from 120 to 10ms.
* display: disable DSC compression for 1080 (like 7T) only.
* input: improve gesture and FOD performance. 
* Boeffla wakelock blocker: bring back, doesn't block at all by default.
* Simple LMK: updates from kerneltoast.
* f2fs: potentially reduce likelihood of jitter.
* locking: more backports and performance improvements.
* net: many backports, including bbr2. Additionally switch to bbr2 and fq codel by default.
* Bump toolchain build (llvm master always built within two days).
* Merge upstream 4.14.232
* Other small optimizations.

**v1.0b1**
*Rough changelog from previous alpha in yaap channel:*
* Affine ufshcd to prime. 
* Some sched backports.
* Minor block change for latency.
* Bump llvm master toolchain (built today).
* Bring in msm simple thermal:
  * Update for our freq table.
  * Attempt to mitigate sooner using more efficient frequencies.
  * Utilize 1708mhz on little cores early on since 1780 isn't much faster, and 1708 is much more efficient.
  * Bias big cluster toward efficient max frequencies sooner, since big cluster contributes the most to temperatures, but the least to jank in high contention situations.
