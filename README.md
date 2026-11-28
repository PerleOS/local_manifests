<!--
SPDX-FileCopyrightText: The PerleOS Project
SPDX-License-Identifier: Apache-2.0
-->

# PerleOS local manifests

`repo` local manifests that add the PerleOS repositories to a LineageOS 24 checkout, for the Lenovo Tab P11 Pro TB-J706F (`j706f`).
The LineageOS manifest is used unchanged, except for the LineageOS projects that PerleOS forks replace (see Contents).

## Use

```sh
repo init -u https://github.com/LineageOS/android -b lineage-24.0 --git-lfs --depth=1
git clone -b lineage-24.0 https://github.com/PerleOS/local_manifests .repo/local_manifests
repo sync -c --no-tags --optimized-fetch
```

`repo` reads every `*.xml` file in `.repo/local_manifests/` after the main manifest.
To update, run `git -C .repo/local_manifests pull` and then `repo sync` again.

## Contents

| Path | Repository | Purpose |
|---|---|---|
| `device/lenovo/pearl-common` | `android_device_lenovo_pearl-common` | Tree shared by the pearl family (TB-J706F, TB-J706L) |
| `device/lenovo/j706f` | `android_device_lenovo_j706f` | TB-J706F (Wi-Fi, ROW) device tree |
| `frameworks/base` | `android_frameworks_base` | Fork of the LineageOS project: microG signature spoofing on user builds, SystemUI desktop windowing crash fix |
| `packages/modules/Bluetooth` | `android_packages_modules_Bluetooth` | Fork of the LineageOS project: LE connection fix for the WCN3990 Bluetooth controller |
| `packages/apps/Aperture` | `android_packages_apps_Aperture` | Fork of the LineageOS project: camera access toggle handling for the stock camera HAL |
| `kernel/lenovo/sm6150` | `android_kernel_lenovo_sm6150` | Kernel source |
| `vendor/perleos` | `android_vendor_perleos` | PerleOS product configuration and branding |
| `vendor/microg` | `android_vendor_microg` | microG and F-Droid prebuilts |
| `vendor/lenovo/pearl-common` | `proprietary_vendor_lenovo_pearl-common` (ThePackrats) | Proprietary files of the pearl family, from the stock S620150 ROW firmware |

The PerleOS and ThePackrats projects track branch `lineage-24.0`. The `lineage-21.0` forks of `packages/overlays/Lineage`, `packages/apps/Trebuchet` and `packages/apps/ThemePicker` are not used on `lineage-24.0`, which takes those projects from LineageOS unchanged.

The proprietary files (`vendor/lenovo/*`) come from the [ThePackrats](https://github.com/ThePackrats) organization, mirrored at https://gitlab.com/ThePackrats. Those repositories hold only the output of the device tree's `extract-files.py` run on the stock S620150 ROW firmware, and can be rebuilt from it.

## Why not `breakfast` / roomservice

`roomservice.py` only looks up device repositories in the LineageOS organization. Because of that, `breakfast j706f` cannot fetch the PerleOS trees on its own. Sync this manifest first; after that, `breakfast` finds the trees already in place.

## Licence

Apache-2.0, see `LICENSE`.
