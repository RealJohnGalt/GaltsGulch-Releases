# Changelog

**v1.122**
 * Updated clang 11 master build.
 * Merged in linux 4.9.218.
 * Merged in new April security patch.

**v1.121**
 * Updated clang 11 master build.
 * Dropped some binutils tools in favor of llvm tools.
 * Optimizing specifically for little cluster now (big cluster doesn't need the help). On clang we can only optimize for a single target unlike gcc.

**v1.120**
 * Merged 4.9.217.
 * Updated clang 11 master build.
 * Better implemented Polly optimizations.

**v1.119**
 * Merged 4.9.216.

**v1.118**
 * Some overhead optimizations by kdrag0n and sultanxda.
 * Fixes to build with current clang 11 (master). And built with current clang 11.
 * Enabled polly optimizations.

**v1.117**
 * Merged the large March release.

**v1.116**
 * Merged in 4.9.215.
 * Updated to latest AnyKernel3, includes addon.d-v2 support.

**v1.115**
 * LOS Support: Now should support any rom available for bonito/sargo.
 * Switch timer to 1000hz: There seems to be a kernel bug around 300hz. We dropped to 250hz from 300hz awhile ago, because 250hz decreased overhead and improved battery life. 1000hz is actually lower overhead than 250hz.

**v1.114**
 * Merged in 4.9.214.
 * Brought back dimmer driver variant.

**v1.113**

 * Merged in Feb patch.
 * Merged in 4.9.213.
 * Removed "supported" version check from anykernel install script.

**v1.112**

 * Discontinued dimmer driver variant (json now points to standard build).
 * Merged in 4.9.212.

**v1.111**

* Merged back in upstream linux kernel, updated to 4.9.210 (thanks to Shag for pointing out the revert for the "youtube issue").

**v1.110**

* Reverted back to 4.9.206 until the upstream pelt issue for us is discovered and fixed on our part. This fixes the video playback panic/reboot.

**v1.109**

* Merged in new monthly patch.
* Should have fixed random reboots.
* Ensure ramoops is preserved on panic/reboot (accidentally broke during previous merge resolution).
