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

## [2.0.22] 2025/09/08 - Max Harris
## Changed
- Broker:
    - Windows: Fix broker crash on startup if using log_dest stdout
    - Bridge: Fix idle_timeout never occurring for lazy bridges.
    - Fix case where max_queued_messages = 0 was not treated as unlimited. Closes #3244.
    - Fix --version exit code and output. Closes #3267.
    - Fix crash on receiving a $CONTROL message over a bridge, if per_listener_settings is set true and the bridge is carrying out topic remapping. Closes #3261.
    - Fix incorrect reference clock being selected on startup on Linux. Closes #3238.
    - Fix reporting of client disconnections being incorrectly attributed to "out of memory". Closes #3253.
    - Fix compilation when using WITH_OLD_KEEPALIVE. Closes #3250.
    - Add Windows linker file for the broker to the installer. Closes #3269.
    - Fix Websockets PING not being sent on Windows. Closes #3272.
    - Fix problems with secure websockets. Closes #1211.
    - Fix crash on exit when using WITH_EPOLL=no. Closes #3302.
    - Fix clients being incorrectly expired when they have keepalive == max_keepalive. Closes #3226, #3286.

## [2.0.21] 2025/03/27 - Max Harris
### Changed
- Broker:
    - Fix clients sending a RESERVED packet not being quickly disconnected. Closes #2325.
    - Fix bind_interface producing an error when used with an interface that has an IPv6 link-local address and no other IPv6 addresses. Closes #2696.
    - Fix mismatched wrapped/unwrapped memory alloc/free in properties. Closes #3192.
    - Fix allow_anonymous false not being applied in local only mode. Closes #3198.
    - Add retain_expiry_interval option to fix expired retained message not being removed from memory if they are not subscribed to. Closes #3221.
    - Produce an error if invalid combinations of cafile/capath/certfile/keyfile are used. Closes #1836. Closes #3130.
    - Backport keepalive checking from develop to fix problems in current implementation. Closes #3138.
- Client library:
    - Fix potential deadlock in mosquitto_sub if -W is used. Closes #3175.
- Apps:
    - mosquitto_ctrl dynsec now also allows -i to specify a clientid as well as -c. This matches the documentation which states -i. Closes #3219.
    - Fix threads linking on Windows for static libmosquitto library Closes #3143
- Build:
    - Fix Windows builds not having websockets enabled.
    - Add tzdata to docker images
- Tests:
    - Fix 08-ssl-connect-cert-auth-expired and 08-ssl-connect-cert-auth-revoked tests when under load. Closes #3208.

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
