# Kernel Versions - The Official 

by macuz-apl | 2 December 2025



## The Ultimate Reference of Linux Kernel Archives

#### 1- The website

​	https://www.kernel.org

| Protocol | Location                      | Remarks             |
| -------- | ----------------------------- | ------------------- |
| HTTP     | https://www.kernel.org/pub/   | mainline/stable/LTS |
| GIT      | https://git.kernel.org/       | collection of gits  |
| RSYNC    | rsync://rsync.kernel.org/pub/ |                     |

#### 2- The stream of kernels

| Stream     | Version       | Release Date | The tarball                                                  | Remarks                                |
| ---------- | ------------- | ------------ | ------------------------------------------------------------ | -------------------------------------- |
| mainline   | 6.18          | 2025-11-30   | [tarball](https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.18.tar.xz) | maintained by Linus Torvalds           |
| stable     | 6.17.10       | 2025-12-01   | [tarball](https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.17.10.tar.xz) | After each mainline kernel is released |
| longterm   | 6.12.60       | 2025-12-01   | [tarball](https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.12.60.tar.xz) | LTS with backported bugfix             |
| longterm   | 6.6.118       | 2025-12-01   | [tarball](https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.6.118.tar.xz) | LTS with backported bugfix             |
| longterm   | 6.1.158       | 2025-10-29   | [tarball](https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.1.158.tar.xz) | LTS with backported bugfix             |
| longterm   | 5.15.196      | 2025-10-29   | [tarball](https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.15.196.tar.xz) | LTS with backported bugfix             |
| longterm   | 5.10.246      | 2025-10-29   | [tarball](https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.10.246.tar.xz) | LTS with backported bugfix             |
| longterm   | 5.4.301       | 2025-10-29   | [tarball](https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.4.301.tar.xz) | LTS with backported bugfix             |
| linux-next | next-20251201 | 2025-12-01   |                                                              |                                        |

#### 3- Comments of the Streams above

There are several main categories into which kernel releases may fall:

- Prepatch

  Prepatch or "RC" kernels are mainline kernel pre-releases that are mostly aimed at other kernel developers and Linux enthusiasts. They must be compiled from source and usually contain new features that must be tested before they can be put into a stable release. Prepatch kernels are maintained and released by Linus Torvalds.

- Mainline

  Mainline tree is maintained by Linus Torvalds. It's the tree where all new features are introduced and where all the exciting new development happens. New mainline kernels are released every 9-10 weeks.

- Stable

  After each mainline kernel is released, it is considered "stable." Any bug fixes for a stable kernel are backported from the mainline tree and applied by a designated stable kernel maintainer. There are usually only a few bugfix kernel releases until next mainline kernel becomes available -- unless it is designated a "longterm maintenance kernel." Stable kernel updates are released on as-needed basis, usually once a week.

- Longterm

  There are usually several "longterm maintenance" kernel releases provided for the purposes of backporting bugfixes for older kernel trees. Only important bugfixes are applied to such kernels and they don't usually see very frequent releases, especially for older trees.

  | Version | Maintainer                       | Released   | Projected EOL |
  | ------- | -------------------------------- | ---------- | ------------- |
  | 6.12    | Greg Kroah-Hartman & Sasha Levin | 2024-11-17 | Dec, 2026     |
  | 6.6     | Greg Kroah-Hartman & Sasha Levin | 2023-10-29 | Dec, 2026     |
  | 6.1     | Greg Kroah-Hartman & Sasha Levin | 2022-12-11 | Dec, 2027     |
  | 5.15    | Greg Kroah-Hartman & Sasha Levin | 2021-10-31 | Dec, 2026     |
  | 5.10    | Greg Kroah-Hartman & Sasha Levin | 2020-12-13 | Dec, 2026     |
  | 5.4     | Greg Kroah-Hartman & Sasha Levin | 2019-11-24 | Dec, 2025     |

#### 4- How to interpret Linux Kernel version numbers?

Linux kernel version numbers are typically presented in a `w.x.y-z` format, though the interpretation has evolved over time.

Here's a breakdown of the current common interpretation:

- `w.x` (Main Kernel Version): 

  The first two numbers together represent the main kernel version. This pair increments when significant changes or a new development cycle begins. For example, the jump from 5.x to 6.x indicates a new major series.

- `y` (Patch Release): 

  This number indicates a patch release within the `w.x` series. It increments for bug fixes, security updates, and minor improvements.

- `z` (ABI Number or Specific Build Information): 

  This component can vary in meaning. It often relates to the Application Binary Interface (ABI) or can signify specific build details, such as a distribution-specific patch level or a release candidate suffix (e.g., `-rcN`).

## The End

