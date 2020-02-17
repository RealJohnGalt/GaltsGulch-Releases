# Changelog

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
