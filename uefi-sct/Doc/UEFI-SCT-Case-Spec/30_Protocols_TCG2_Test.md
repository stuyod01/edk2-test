**UEFI SCT II Case Specification**

## 30.1 TCG2 Protocol Test

**Reference Document:**

_EFI Protocol Specification_, EFI\_TCG2\_PROTOCOL Chapter 6

Tests in this chapter support TPMs based TCG PC Client Platform TPM Profile Specification for TPM 2.0.

### 30.1.1 GetCapability()

| **Number** | **GUID** | **Assertion** | **Test Description** |
| --- | --- | --- | --- |
| 30.1.1.1 | 0xca93b02a, 0xe897, 0x4400, 0x81, 0x38, 0xc8, 0xa8, 0xcb, 0x2f, 0xc1, 0xed | EFI\_TCG2\_PROTOCOL. GetCapability() - GetCapabilty()returns EFI\_INVALID\_PARAMTERwith NULL pointer Capability Struct Passed in. | Call GetCapability()with NULL for capability struct pointer.<ol type="a"><li>Verify Status returned == EFI\_INVALID\_PARAMETER.</ol>  |
| 30.1.1.2 | 0xda8821d9, 0x3d2c, 0x4698, 0x8c, 0xd5, 0x0f, 0x0c, 0x82, 0x94, 0x1d, 0x0c | EFI\_TCG2\_PROTOCOL.GetCapability() –GetCapability() shall populate the included structure elements and return with a Status of EFI\_BUFFER\_TOO\_SMALL when structure size is set to less than the size of EFI\_TCG\_BOOT\_SERVICE\_CAPABILITY.  | Invoke GetCapability() with ProtocolCapability.Size set to only include StructureVersion and ProtocolVersion.<ol type="a"><li>Verify that StructureVersion->Major == 1<li>Verify that StructureVersion->Minor == 1<li>Verify that ProtocolVersion->Major == 1<li>Verify that ProtocolVersion->Minor == 1<li>Verify Status returned == EFI\_BUFFER\_TOO\_SMALL.<li>Verify returned Size equal to size of the EFI\_TCG2\_BOOT\_SERVICE\_CAPABILITY up to and including the vendor ID field. </ol> |
| 30.1.1.3 | 0xfdee7001, 0x7e28, 0x4e35, 0x99, 0x66, 0x98, 0x0b, 0xeb, 0xba, 0xf1, 0x57 | EFI\_TCG2\_PROTOCOL. GetCapability() - GetCapability() shall populate all structure elements and return with a Status of EFI\_SUCCESS when the structure size includes all of the EFI\_TCG\_BOOT\_SERVICE\_CAPABILITY structure.  | Invoke GetCapabilty()with Protocol Capability. Size set to sizeof(EFI\_TCG2\_BOOT\_SEVICE\_CAPABILTY)<ol type="a"><li>Verify that StructureVersion->Major == 1<li>Verify that StructureVersion->Minor == 1<li>Verify that ProtocolVersion->Major == 1<li>Verify that ProtocolVersion->Minor == 1<li>Verify that HashAlgorithmBitmapincludes SHA256<li>Verify that SupportedEventLogs is EFI\_TCG2\_EVENT\_LOG\_FORMAT\_TCG\_2<li>Verify that ~ActivePcrBanks &amp; HashAlgorithmBitMap == 0<li>Verify that NumberofPcrBanks is at least 1<li>Verify that ActivePcrBanks includes SHA256, SHA384, or SHA512<li>Verify returned Status == EFI\_SUCCESS.</ol> |

### 30.1.2 GetActivePcrBanks()

| **Number** | **GUID** | **Assertion** | **Test Description** |
| --- | --- | --- | --- |
| 30.1.2.1 | 0x7a1e79a3, 0x4064, 0x4372, 0xbb, 0x64,0x55, 0xb8, 0xf2, 0xa5, 0xa3, 0x26 | EFI\_TCG2\_PROTOCOL. GetActivePcrBanks() - GetActivePcrBanks() returns EFI\_INVALID\_PARAMETERwith NULL pointer Passed in. | Invoke GetActivePcrBanks()with ActivePcrBanks=NULL.<ol type="a"><li>Verify Status returned == EFI\_INVALID\_PARAMETER.</ol>  |
| 30.1.2.2 | 0xb0e717c4, 0xb1e2, 0x49f7, 0xb2, 0xd7,0x60, 0x58,0x97, 0x7d, 0x09, 0x2c | EFI\_TCG2\_PROTOCOL. GetActivePcrBanks() - GetActivePcrBanks()should return with EFI\_SUCCESSand have SHA256/384/512 Algorithms in its Bitmap. | 1. Invoke GetActivePcrBanks()with valid ActivePcrBanks buffer. Should return EFI\_SUCCESS.<ol type="a"><li>Verify Status returned == EFI\_SUCCESS<li>Verify that returned ActivePcrBanks bitmap includes SHA256, SHA384, or SHA512.c. Verify that returned ActivePcrBanks bitmap matches one returned by GetCapabilty().</ol> |

### 30.1.3 HashLogExtendEvent()

| **Number** | **GUID** | **Assertion** | **Test Description** |
| --- | --- | --- | --- |
| 30.1.3.1 | 0xa8e1b5e6, 0xfc09, 0x461c, 0xb0, 0xe9, 0x2a, 0x49, 0xcd, 0x25, 0xc1, 0x24 | EFI\_TCG2\_PROTOCOL. HashLogExtendEvent() - HashLogExtendEvent()Test with NULL DataToHash Pointer should return EFI\_INVALID\_PARAMETER.  | Invoke HashLogExtendEvent()with DataToHash=NULL.<ol type="a"><li>Verify Status returned == EFI\_INVALID\_PARAMETER.</ol> |
| 30.1.3.2 | 0x26f04a9b, 0x7b7a, 0x4f47, 0xbe, 0xa8, 0xb1, 0xa6, 0x02, 0x65, 0x19, 0x8a | EFI\_TCG2\_PROTOCOL. HashLogExtendEvent() - HashLogExtendEvent()Test with NULL EfiTcgEvent Pointer should return EFI\_INVALID\_PARAMETER. | Invoke HashLogExtendEvent() with EfiTcgEvent=NULL<ol type="a"><li>Verify Status returned == EFI\_INVALID\_PARAMETER.</ol> |
| 30.1.3.3 | 0x4d1d9985, 0x91e2, 0x4948, 0x89, 0x16, 0xbb, 0x98, 0x13, 0x62, 0x39, 0x1d | EFI\_TCG2\_PROTOCOL. HashLogExtendEvent() - HashLogExtendEvent()Test with Event.Size &lt; Event.Header.HeaderSize + sizeof(UINT32) should return EFI\_INVALID\_PARAMETER. | Invoke HashLogExtendEvent() with EfiTcgEvent.Size is less than EfiTcgEvent.Header.HeaderSize + sizeof(UINT32). <ol type="a"><li>Verify Status returned == EFI\_INVALID\_PARAMETER.</ol>  |
| 30.1.3.4 | 0xfb59cab7, 0x4f8c, 0x4ded, 0xa4, 0x1c, 0xc8, 0x41, 0x20, 0x1c, 0x37, 0x22 | EFI\_TCG2\_PROTOCOL. HashLogExtendEvent() - HashLogExtendEvent()Test with PCRIndex > 23 should return EFI\_INVALID\_PARAMETER. | Invoke HashLogExtendEvent with EfiTcgEvent.Header.PCRIndex=24.<ol type="a"><li>Verify Status returned == EFI\_INVALID\_PARAMETER.</ol> |
| 30.1.3.5 | 0x0363d22f, 0xc66a, 0x4872, 0xa5, 0x46, 0x06, 0x7f, 0x6a, 0x0d, 0xdb, 0xcd | EFI\_TCG2\_PROTOCOL. HashLogExtendEvent() - HashLogExtendEvent() Test with valid parameters should return EFI\_SUCCESS. | Invoke HashLogExtendEvent() with:<ul><li>DataToHash = &quot;The quick brown fox jumps over the lazy dog&quot;<li>PCRIndex = 16<li>EventType = EV\_POST\_CODE<li>Event data = &quot;TCG2 Protocol Test&quot;</ul><ol type="a"><li>Verify Status returned == EFI\_SUCCESS.</ol> |
| 30.1.3.6 | 0x9cd6d636, 0x603a, 0x4b78, 0x80, 0xa3, 0xa3, 0xb9, 0xcc, 0x6a, 0x0b, 0x08 | EFI\_TCG2\_PROTOCOL. HashLogExtendEvent() - HashLogExtendEvent()Test Handling of PE\_COFF\_IMAGE flag. | Invoke HashLogExtendEvent() with:<ul><li>DataToHash = &quot;The quick brown fox jumps over the lazy dog&quot;<li>PCRIndex = 16<li>EventType = EV\_POST\_CODE<li>Event data = &quot;TCG2 Protocol Test&quot;<li>Flags = PE\_COFF\_IMAGE</ul><ol type="a"><li>Verify Status returned == EFI\_UNSUPPORTED.</ol> |

### 30.1.4 GetEventLog()

| **Number** | **GUID** | **Assertion** | **Test Description** |
| --- | --- | --- | --- |
| 30.1.4.1 | 0xfc80408e, 0x9a3c, 0x4054, 0x96, 0xf9, 0x31, 0x23, 0x35, 0xc2, 0x31, 0x35 | EFI\_TCG2\_PROTOCOL. GetEventLog() - GetEventLog() should return EFI\_INVALID\_PARAMTER when passed in invalid EventLog Format. | Invoke GetEventLog() with invalid EventLogFormat.<ol type="a"><li>Verify Status returned == EFI\_SUCCESS.<ol>  |
| 30.1.4.2 | 0x45fa1a42, 0x912a, 0x5124, 0x84, 0xf4, 0x41, 0x67, 0xab, 0xb5, 0x89, 0x90 | EFI\_TCG2\_PROTOCOL. GetEventLog() - GetEventLog() shall return EFI\_SUCCESS when a valid EventLogFormat is passed in.  | Invoke GetEventLog() with EventLogFormat=EFI\_TCG2\_EVENT\_LOG\_FORMAT\_TCG\_2. Should return EFI\_SUCCESS.<ol type="a"><li>Verify Status returned == EFI\_SUCCESS.</ol> |
| 30.1.4.3 | 0x1689bc3a, 0x2298, 0xa116, 0x28, 0x4c, 0xc1, 0xdd, 0xaa, 0xd8, 0xef, 0x51 | EFI\_TCG2\_PROTOCOL. GetEventLog() - GetEventLog() should return correct EventLogHeader | Verify that the returned event log is present at EventLogLocation address by verifying event log header.  |
| 30.1.4.4 | 0x126a789a, 0x1932, 0x3234, 0x21, 0xab, 0x42, 0x64, 0x8a, 0x7b, 0x63, 0x76 | EFI\_TCG2\_PROTOCOL. GetEventLog() - GetEventLog() should record Event from Test 0x0363d22f as last EventLogEntry. | Verify that an event log entry is present at EventLogLastEntry by verifying the last entry. The last entry should be the one added with the HashLogExtendEvent in test 0x0363d22f:<ol type="a"><li>Verify TCG\_PCR\_EVENT2.PCRIndex = 16<li>Verify TCG\_PCR\_EVENT2.EventType = EV\_POST\_CODE<li>Verify TCG\_PCR\_EVENT2.Digests.Count = [must equal number of active PCR banks]</ol> |

### 30.1.5 SubmitCommmand() 
| **Number** | **GUID** | **Assertion** | **Test Description** |
| --- | --- | --- | --- |
| 30.1.5.1 | 0x3aac8b9a, 0x312a, 0x4dcf, 0x12, 0x76, 0x54, 0x55, 0x32, 0xcd, 0x3a, 0xea | EFI\_TCG2\_PROTOCOL.SubmitComand() - SubmitCommand() shall populate the response buffer and return with a status of EFI\_SUCCESS when valid command parameters are passed in.  | Invoke SubmitCommand() with a command buffer containing Command TPM2\_HASH Command, and Data to Hash is "The quick brown fox jumps over the lazy dog".  <ol type="a"> <li>Verify Status returned == EFI\_SUCCESS.  <li>Verify returned outHash matches expected result </ol> |

### 30.1.6 SetActivePcrBanks()

| **Number** | **GUID** | **Assertion** | **Test Description** |
| --- | --- | --- | --- |
| 30.1.6.1  | 0x48586d48, 0xa8a4, 0x4129, 0x99, 0x8e, 0x78, 0x62, 0x3a, 0xac, 0x43, 0x6a | EFI\_TCG2\_PROTOCOL.SetActivePcrBanks () - SetActivePcrBanks() should return EFI\_INVALID\_PARAMETER with bitmap value of zero. | Invoke SetActivePcrBanks() with ActivePcrBanks=0 <ol><li> Verify Status returned == EFI\_INVALID\_PARAMETER.</ol> |
| 30.1.6.2 | 0x1ed1b506, 0xc3c9, 0x45ea, 0xbf, 0x1e, 0x9b, 0xf2, 0x87, 0x08, 0x4e, 0x17 | EFI\_TCG2\_PROTOCOL.SetActivePcrBanks () - SetActivePcrBanks() should return EFI\_INVALID\_PARAMETER with an ActivePcrBanks value with a bit set but not defined in HashAlgorithmBitMap.  | <ol><li>Invoke GetCapabilty() to retrieve HashAlgorithmBitmap.<li>Invoke SetActivePcrBanks() with a bit set that is not in HashAlgorithmBitMap <ol type="a"><li>Verify Status returned == EFI\_INVALID\_PARAMETER</li></ol></li></ol>. |
| 30.1.6.3 | 0x2923c2b2, 0x5a83, 0x4977, 0xbd, 0xed, 0x78, 0xb0, 0xd8, 0xbb, 0xcc, 0x77 | EFI\_TCG2\_PROTOCOL.SetActivePcrBanks () - SetActivePcrBanks() should return EFI\_SUCESS for all bank permutations defined in HashAlgorithmmBitmap.  | Using the HashAlgorithmBitMap from 30.1.6.2, invoke SetActivePcrBanks() with ActivePcrBanks from all permutations of banks reported in HashAlgorithmBitMap retrieved via GetCapabilities(). For each Permutation: <ol type="a"><li>Verify Status returned == EFI\_SUCCESS.</ol> |
| 30.1.6.4 | 0x181bc213, 0x4512, 0x47ea, 0x8a, 0xb6, 0x44, 0x76, 0xbf, 0x9a, 0x44, 0x6d | EFI\_TCG2\_PROTOCOL.SetActivePcrBanks () - Verify that the ActivePcrBanks value set by SetActivePcrBanks() is active following a reboot.  | <ol><li>Identify the first algorithm in HashAlgorithmBitMap retrieved in test 30.1.6.3<li>Invoke ActivePcrBanks() to set the ActivePcrBanks to the first algorithm. <li> Reboot the system<li>Invoke GetActivePcrBanks(). Should return EFI\_SUCCESS, and ActivePcrBanks should be equal to the value set in step #2 |

