**UEFI SCT II Case Specification**

-
# Introduction

-
## Overview

This document provides detailed information for each assertion in the UEFI SCT fundamental service and protocol tests. This document can be used as a reference on case assertion for UEFI SCT users.

**Reference Documents**

- _UEFI Specification_
- _--_ indicates current and past UEFI specifications, unless specific versions are noted

- _UEFI SCT Getting Started_
- _UEFI SCT User Guide_

-

| 5.26.2.7.71 | 0x732738e8, 0x1ff1, 0x4f3a, 0xa0,0xc8, 0x38,0x81,0x1d,0x15,0x92,0x83 | **EFI\_MTFTP4\_PROT**** OCOL.ReadFile() **-** ReadFile() **must return** EFI\_PROTOCOL\_UNREACHABLE **when receive an ICMP protocol unreachable packet. | 1. Call** EFI\_MTFTP4\_SERVICE\_BINDING\_PROTOCOL.CreateChild() **to createa new** EFI\_MTFTP4\_PROTOCOL **child handle.2. Call** EFI\_MTFTP4\_PROTOCOL.Configur****e()** with all valid parameters.3. Call **EFI\_MTFTP4\_PROTOCOL.ReadFile**** () **with all valid parameters. OS sideshould capture the packet sent fromEUT side.4. Configure Host side to send back anICMP protocol unreachable packet and the return status should be** EFI\_PROTOCOL\_UNREACHABLE **.5. Call** EFI\_MTFTP4\_SERVICE\_BINDING\_PROTOCOL.DestroyChild() **todestroy the newly created** EFI\_MTFTP4\_PROTOCOL** childhandle and clean up the environment. |
| --- | --- | --- | --- |
| 5.26.2.7.72 | 0xd1c4e1e8, 0x1099, 0x4646, 0xb7,0xc9, 0x64,0x7e, 0x65,0xc3, 0x82,0x30 | **EFI\_MTFTP4\_PROT**** OCOL.ReadFile() **-** ReadFile() **must return** EFI\_PORT\_UNREACHABLE **when receive an ICMP port unreachable packet. | 1. Call** EFI\_MTFTP4\_SERVICE\_BINDING\_PROTOCOL.CreateChild() **to createa new** EFI\_MTFTP4\_PROTOCOL **child handle.2. Call** EFI\_MTFTP4\_PROTOCOL.Configur****e()** with all valid parameters.3. Call **EFI\_MTFTP4\_PROTOCOL.ReadFile**** () **with all valid parameters. OS sideshould capture the packet sent fromEUT side.4. Configure Host side to send back anICMP port unreachable packet and the return status should be** EFI\_PORT\_UNREACHABLE **.5. Call** EFI\_MTFTP4\_SERVICE\_BINDING\_PROTOCOL.DestroyChild() **todestroy the newly created** EFI\_MTFTP4\_PROTOCOL** childhandle and clean up the environment. |

-

-
## System Hang

If the system hangs in any of tests, the UEFI SCT framework records a failure assertion in the test report and skips this test after a system restart.

| **Number** | **GUID** | **Assertion** | **Test Description** |
| --- | --- | --- | --- |
| 5.20.1.1.1 | 0xde687a18, 0x0bbd, 0x4396, 0x85, 0x09, 0x49, 0x8f, 0xf2, 0x32, 0x34, 0xf1 | System hangs or stops.. | The name of the test which causes the system hang can be found in the test report. |

June 2017 1