# Changelog

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
