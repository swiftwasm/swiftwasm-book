## Continuous Integration

We use GitHub Actions to build and run tests continuously.
(But jobs without cache takes 2~3 hours to be completed)

Currently (2020/10/4), we only run stdlib tests on Linux CI because the job execution time and storage reached GitHub Actions limits.

References:

- [swiftwasm/swift#160](https://github.com/swiftwasm/swift/pull/160#issuecomment-586537014)
- [swiftwasm/swift#232](https://github.com/swiftwasm/swift/pull/232)


### Nightly distribution

We distribute latest `swiftwasm` branch and `swiftwasm-release/5.3` branch toolchains at UTC 00:00 everyday. You can check the latest toolchain at [GitHub Release Page](https://github.com/swiftwasm/swift/releases)

## Cache

Compilation time of LLVM and the Swift toolchain is very long, so we recommend to cache the artifacts using `sccache`. [The "Getting started" guide](https://github.com/apple/swift/blob/main/docs/HowToGuides/GettingStarted.md#the-roles-of-different-tools) from the upstream toolchain provides more details about the use of `sccache`.


### References

- [mozilla/sccache](https://github.com/mozilla/sccache)
- [Use sccache to cache build artifacts](https://github.com/apple/swift/blob/master/docs/DevelopmentTips.md#use-sccache-to-cache-build-artifacts)
