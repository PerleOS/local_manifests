<!--
SPDX-FileCopyrightText: The PerleOS Project
SPDX-License-Identifier: Apache-2.0
-->

# PerleOS local manifests

`repo` local manifests that add the PerleOS repositories to a LineageOS 21 checkout, for the Lenovo Tab P11 Pro TB-J706F (`j706f`).
The LineageOS manifest is used unchanged, except for the LineageOS projects that PerleOS forks replace (see Contents).

## Use

```sh
repo init -u https://github.com/LineageOS/android -b lineage-21.0 --git-lfs --depth=1
git clone -b lineage-21.0 https://github.com/PerleOS/local_manifests .repo/local_manifests
repo sync -c --no-tags --optimized-fetch
```

`repo` reads every `*.xml` file in `.repo/local_manifests/` after the main manifest.
To update, run `git -C .repo/local_manifests pull` and then `repo sync` again.

## Contents

| Path | Repository | Purpose |
|---|---|---|
| `device/lenovo/pearl-common` | `android_device_lenovo_pearl-common` | Tree shared by the pearl family (TB-J706F, TB-J706L) |
| `device/lenovo/j706f` | `android_device_lenovo_j706f` | TB-J706F (Wi-Fi, ROW) device tree |
| `packages/modules/Connectivity` | `android_packages_modules_Connectivity` | Fork of the LineageOS project: BPF loader and network stack fixes for the stock 4.14 kernel |
| `system/bpf` | `android_system_bpf` | Fork of the LineageOS project: BPF loader fix for the stock 4.14 kernel |
| `frameworks/base` | `android_frameworks_base` | Fork of the LineageOS project: microG signature spoofing on user builds |
| `packages/overlays/Lineage` | `android_packages_overlays_Lineage` | Fork of the LineageOS project: declares the Space Grotesk font family |
| `kernel/lenovo/sm6150` | `android_kernel_lenovo_sm6150` | Kernel source |
| `vendor/perleos` | `android_vendor_perleos` | PerleOS product configuration and branding |
| `vendor/microg` | `android_vendor_microg` | microG and F-Droid prebuilts |

All projects track branch `lineage-21.0`.

Not included:

- **Proprietary files** (`vendor/lenovo/*`): these repositories are private. On a machine with access, put a second file such as `.repo/local_manifests/private.xml` next to `perleos.xml`. It should use the `private` remote that the LineageOS manifest already defines.

## Why not `breakfast` / roomservice

`roomservice.py` only looks up device repositories in the LineageOS organization. Because of that, `breakfast j706f` cannot fetch the PerleOS trees on its own. Sync this manifest first; after that, `breakfast` finds the trees already in place.

## Licence

Apache-2.0, see `LICENSE`.
