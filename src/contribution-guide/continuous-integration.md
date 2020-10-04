# Continuous Integration

We use GitHub Actions to build and run tests continuously.
(But jobs without cache takes 2~3 hours to be completed)

Currently (2020/10/4), we only run stdlib tests on Linux CI because the job exection time and storage reached the limit of GitHub Actions.

References:

- [swiftwasm/swift#160](https://github.com/swiftwasm/swift/pull/160#issuecomment-586537014)
- [swiftwasm/swift#232](https://github.com/swiftwasm/swift/pull/232)


## Nightly distribution

We distribute latest `swiftwasm` branch and `swiftwasm-release/5.3` branch toolchains at UTC 00:00 everyday. You can check the latest toolchain at [GitHub Release Page](https://github.com/swiftwasm/swift/releases)