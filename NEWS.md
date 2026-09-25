# 2026

## September

- **Create a `teamsbc-uboot` package** ([#47](https://github.com/teamsbc/distribution/issues/47)).
  Built a custom `teamsbc-uboot` package with per-board subpackages for images.
  This avoids the disk space overhead of shipping all U-Boot images in `/usr`
  and lays the groundwork for TPM2 support ahead of Fedora.

- **Write `IMAGE_ID` and `IMAGE_VERSION` into `os-release` at build time** ([#41](https://github.com/teamsbc/distribution/issues/41)).
  `image-builder` now passes `IMAGE_ID` and `IMAGE_VERSION` through to the RPM
  stage so images and their sysexts can be versioned at build time, making them
  compatible with `systemd-sysupdate`.

- **Label partitions with `IMAGE_VERSION`** ([#49](https://github.com/teamsbc/distribution/issues/49)).
  The `/usr` partition is now labeled with the `IMAGE_VERSION` it was built
  with so that `systemd-sysupdate` can discover and update it.

- **Offline provisioning with `systemd-firstboot` leads to non-functioning system** ([#29](https://github.com/teamsbc/distribution/issues/29)).
  Fixed a bug where using `systemd-firstboot --image` for offline provisioning
  of root password and timezone caused services to fail on boot, leaving the
  system unusable.

- **Only build `rawhide`** ([#51](https://github.com/teamsbc/distribution/issues/51)).
  Narrowed CI to only build rawhide, since the project relies heavily on recent
  upstream releases.

- **Export partitions separately as part of an image build** ([#44](https://github.com/teamsbc/distribution/issues/44)).
  Added support in `image-builder` to export individual partitions (like `/usr`)
  as separate files during the build, enabling sysupdate-compatible update
  delivery.

- **Update `updates.teamsbc.net` through CI** ([#45](https://github.com/teamsbc/distribution/issues/45)).
  CI now automatically publishes built artifacts to `updates.teamsbc.net`,
  closing the loop on the automated update pipeline.

## August

- **Rename `standard` to `lhotse`** ([#37](https://github.com/teamsbc/distribution/issues/37), [#33](https://github.com/teamsbc/distribution/issues/33)).
  Adopted abstract naming for variants (Himalayan mountains) instead of names
  that imply a value judgement. The `standard` variant is now called `lhotse`.

- **Sign RPMs** ([#4](https://github.com/teamsbc/distribution/issues/4)).
  RPMs in our repositories are now signed and the public keys are included in
  the repos package.

- **Dracut does not allow for `root=dissect`** ([#35](https://github.com/teamsbc/distribution/issues/35)).
  Submitted an upstream fix to dracut to accept all `root=` values that systemd
  supports, not just `gpt-auto`. Needed for the Makalu image-based variant.

- **Correctly handle cmdline in `60-ukify`** ([#40](https://github.com/teamsbc/distribution/issues/40)).
  Fixed an upstream systemd issue where the `60-ukify` kernel-install plugin
  didn't detect running in a container, resulting in the wrong command line
  being embedded in the UKI.

- **No non-default tooling for kernel filename** ([#39](https://github.com/teamsbc/distribution/issues/39)).
  Adjusted `kernel-install` upstream to support configurable entry names so
  UKIs can be named with a `VERSION_ID` for sysupdate compatibility.

- **Create sysupdate update infrastructure** ([#42](https://github.com/teamsbc/distribution/issues/42)).
  Set up a dedicated subdomain to serve systemd-sysupdate updates including
  split-out `/usr` partitions, UKIs, and sysexts.

- **Support Fedora 46** ([#46](https://github.com/teamsbc/distribution/issues/46)).
  Added Fedora 46 support after the Fedora branch point and dropped Fedora 44.

## July

- **`systemd-repart` fails to start on build** ([#31](https://github.com/teamsbc/distribution/issues/31)).
  Fixed a disk image alignment mismatch between `image-builder` and
  `systemd-repart` that caused repart to fail when the image wasn't resized
  after building. Addressed upstream and enabled the fix.

## June

- **Use `systemd-boot`** ([#9](https://github.com/teamsbc/distribution/issues/9)).
  Switched from `grub2` to `systemd-boot` as the bootloader, requiring
  upstream work in `image-builder` to support the U-Boot to EFI to
  systemd-boot transition.

- **Identical BLS entry titles for different kernels** ([#23](https://github.com/teamsbc/distribution/issues/23)).
  Fixed an issue where multiple installed kernels produced identical BLS entry
  titles, making it hard to tell them apart.

- **Raspberry Pi 4 does not boot** ([#25](https://github.com/teamsbc/distribution/issues/25)).
  Fixed a boot regression on Raspberry Pi 4 caused by the BLS prefix not being
  adjusted properly after the `systemd-boot` changes. Required an upstream
  `image-builder` fix.

- **Copy firmware from build tree** ([#8](https://github.com/teamsbc/distribution/issues/8)).
  Changed to install `uboot-images-armv8` in the build tree and copy from there
  instead of installing it into the system tree, saving significant disk space.

- **Create a `teamsbc-selinux-policies` package** ([#28](https://github.com/teamsbc/distribution/issues/28)).
  Created a package to ship temporary SELinux policies for components that don't
  have correct policies yet in Fedora, avoiding the need to patch and maintain
  each affected package downstream.

- **Enable boot counting** ([#27](https://github.com/teamsbc/distribution/issues/27)).
  Enabled `systemd-boot` automatic boot assessment by shipping an
  `/etc/kernel/tries` file in the release package.

- **Include `wpa_supplicant` in standard build** ([#26](https://github.com/teamsbc/distribution/issues/26)).
  Added `wpa_supplicant` to images for devices with wifi controllers, needed by
  `systemd-networkd` for WLAN credential configuration.

- **Provide `standard-rpi5` artifacts** ([#30](https://github.com/teamsbc/distribution/issues/30)).
  Added Raspberry Pi 5 as a supported board now that upstream support is
  sufficiently mature.

## May

- **Implement dated repositories** ([#15](https://github.com/teamsbc/distribution/issues/15)).
  Implemented dated builds for packages (matching what was already done for
  artifacts), including a cleanup workflow for old builds.

- **Consider dropping `legacy` variant** ([#21](https://github.com/teamsbc/distribution/issues/21)).
  Dropped the `legacy` variant, which targeted boards without GPT table support
  and differed significantly from the standard variant.

- **Create a `teamsbc-config-*` package** ([#7](https://github.com/teamsbc/distribution/issues/7)).
  Created a config package containing `systemd-repart` files and other default
  service configurations for GPT-based image types.

- **Omit `root=` kernel argument configuration** ([#22](https://github.com/teamsbc/distribution/issues/22)).
  Stopped writing the `root=` kernel argument so that
  `systemd-gpt-auto-generator` mounts the root partition instead, which
  correctly honors the growfs flag. Required upstream `image-builder` work.

## April

- **Generate HTML indexes** ([#5](https://github.com/teamsbc/distribution/issues/5)).
  Added HTML directory listing generation for artifact and package repositories
  hosted on Cloudflare R2, generated as part of the GitHub Actions workflows.

- **Use `systemd-gpt-auto-generator` where available** ([#2](https://github.com/teamsbc/distribution/issues/2)).
  Switched GPT-based image types from explicit systemd mount units to
  `systemd-gpt-auto-generator` for automatic partition discovery during boot.
  Required upstream `image-builder` work.

- **Don't write `hostname`** ([#18](https://github.com/teamsbc/distribution/issues/18)).
  Stopped manually writing the hostname in image definitions since
  `teamsbc-release` already provides the default hostname via systemd first
  boot.

- **Don't write `/etc/sysconfig/kernel`** ([#17](https://github.com/teamsbc/distribution/issues/17)).
  Stopped writing kernel sysconfig values in image definitions since `grubby`
  now owns the file and writes the appropriate defaults itself.

- **Move `.osbuild-manifest.json` into `meta/` subdirectory** ([#16](https://github.com/teamsbc/distribution/issues/16)).
  Moved osbuild JSON manifests from alongside images into a `meta/`
  subdirectory to reduce clutter in directory listings.

- **Share the index template for artifacts and packages** ([#19](https://github.com/teamsbc/distribution/issues/19)).
  Extracted the duplicated `s3-directory-listing` template into its own
  repository and submoduled it into both artifacts and packages.

## March

- **Figure out if `systemd-timesyncd` is set up correctly** ([#12](https://github.com/teamsbc/distribution/issues/12)).
  Verified that `systemd-timesyncd` works correctly out of the box in the
  artifacts.

- **Drop `sudo` from artifacts** ([#13](https://github.com/teamsbc/distribution/issues/13)).
  Removed `sudo` from the artifacts in favor of the systemd-provided `run0` for
  privilege escalation.

- **Set up CI for the handbook** ([#14](https://github.com/teamsbc/distribution/issues/14)).
  Set up automatic building and deployment of the `mdbook`-based handbook at
  `handbook.teamsbc.org`.

## February

- **`systemd-homed` is broken** ([#1](https://github.com/teamsbc/distribution/issues/1)).
  Fixed a bug where creating a user through the `systemd-homed` first boot
  wizard resulted in a login failure due to `authselect` not being configured to
  unlock the home directory.

- **Split apart `standard` into `standard` and `legacy`** ([#3](https://github.com/teamsbc/distribution/issues/3)).
  Introduced a `legacy` variant for older boards that only support MBR partition
  tables, separate from the GPT-based `standard` variant.
