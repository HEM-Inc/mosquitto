# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).
For build level release notes see https://github.com/mtconnect/cppagent/

---
## Types of changes
### `Added` for new features.
### `Changed` for changes in existing functionality.
### `Deprecated` for soon-to-be removed features.
### `Removed` for now removed features.
### `Fixed` for any bug fixes.
### `Security` in case of vulnerabilities.
---

## [Unreleased] 

## [2.0.20] 2025/01/06 - Max Harris
### Changed
- This is a bugfix release.
- Broker:
    - Fix QoS 1 / QoS 2 publish incorrectly returning "no subscribers". Closes #3128.
    - Open files with appropriate access on Windows. Closes #3119.
    - Don't allow invalid response topic values.
    - Fix some strict protocol compliance issues. Closes #3052.
- Client library:
    - Fix cmake build on OS X. Closes #3125.
- Build:
    - Fix build on NetBSD

## [2.0.19] 2024/10/07 - Max Harris
### Changed
- This is a security and bugfix release.
- Security: - Fix mismatched subscribe/unsubscribe with normal/shared topics. 
    - Fix crash on bridge using remapped topic being sent a crafted packet. 
    - Don't allow SUBACK with missing reason codes in client library.
- Broker: 
    - Fix assert failure when loading a persistence file that contains subscriptions with no client id. 
    - Fix local bridges being incorrectly expired when persistent_client_expiration is in use. 
    - Fix use of CLOCK_BOOTTIME for getting time. Closes #3089.
    - Fix mismatched subscribe/unsubscribe with normal/shared topics.
    - Fix crash on bridge using remapped topic being sent a crafted packet.
- Client library: 
    - Fix some error codes being converted to string as "unknown". Closes #2579. 
    - Clear SSL error state to avoid spurious error reporting. Closes #3054. - Fix "payload format invalid" not being allowed as a PUBREC reason code. - Don't allow SUBACK with missing reason codes.

## [2.0.18] 2023/12/27 - Max Harris
### Changed
Approved the init release of the 2.0.18 eclipse-mosquitto version.