# Changelog

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
