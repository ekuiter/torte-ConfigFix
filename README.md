# torte-ConfigFix

*This repository contains a distribution of ConfigFix that extracts feature-model formulas from KConfig files.*

**Why this repository?** The main issue with integrating ConfigFix into torte is its size, because it includes a complete copy of the Linux kernel Git repository, which is several GiB in size.
While this works, it causes long build times and large Docker images.
Thus, we decided to only keep the essential parts needed to compile and execute ConfigFix (in particular, its formula export).

**Which ConfigFix version?** As of 2025-11-28, the latest version of ConfigFix is based off [a patch](https://lkml.org/lkml/2025/2/8/405) to the Linux kernel mailing list, which has been applied to next-20250207.
(Various older versions are available as branches of [delta-one/linux](https://github.com/delta-one/linux/branches/all).)
After the patch was submitted, several changes proposed by kernel developers have been integrated, which are currently upstream [here](https://github.com/delta-one/linux/commit/5eb90f32173b5eaf5dee011dddd1dd0795d2a7d5).
This is also the version we use here for integration into [torte](https://github.com/ekuiter/torte/tree/main/src/docker/configfix).

**How was this repository created?** Thus, to prepare this repository, we downloaded the chosen version of ConfigFix as a [ZIP file](https://github.com/delta-one/linux/archive/5eb90f32173b5eaf5dee011dddd1dd0795d2a7d5.zip), built and executed its Docker container in torte, all the while repeatedly removing files that do not affect the successful compilation of ConfigFix.
We also made the following patch: `echo "SUBARCH := x86" > scripts/subarch.include`
We committed the end result into a new Git repository (the one you're looking at right now) with a single commit, squatting down all history and creating the smallest needed repository to set up ConfigFix.
One might argue that this loses the commit history of the development of ConfigFix, but this history is not available nonetheless, not even in the [delta-one/linux](https://github.com/delta-one/linux/branches/all) repository.

**How can this repository be used?** Because this version of ConfigFix requires PicoSAT to be installed as a dynamic library, it is not prepared to be run out of the box, but rather inside a Docker container prepared by [torte](https://github.com/ekuiter/torte/tree/main/src/docker/configfix).
However, as documented in our [Dockerfile](https://github.com/ekuiter/torte/blob/main/src/docker/configfix/Dockerfile), it is easy to set up PicoSAT and make it available in `LD_LIBRARY_PATH` before running `make cfoutconfig Kconfig=<kconfig-file>`.

## License

The source code of ConfigFix is released under the [GPL v2 license](https://github.com/isselab/configfix/blob/master/LICENSE) ([credits](https://github.com/isselab/configfix/#credits)).
