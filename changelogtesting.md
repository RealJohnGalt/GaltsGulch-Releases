# Changelog

**v1.0b4a20**
* Disable zram, swap, etc again.
* battery: bring back high resolution info.
* config: switch back to 80hz timer for efficiency (bt crackling seems to be fixed).

**v1.0b4a16**
* More various optimizations over a11.
* Re enable zram and friends + related backports (6G ram and heavy usage people complained).
* Revert s2idle: a few users still had wakeup issues.
* msm thermal simple: tweak battery temp range to make more conservative. Results in better warp charging throughput, and uses the battery less as a heatsink when active.

**v1.0b4a11**
* Various optimizations and cleanups.

**v1.0b4a4**
* Bring in klapse.
* Display: ensure ulps is always on, and enable partial update with singleroi.
* Other small changes and optimizations.

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