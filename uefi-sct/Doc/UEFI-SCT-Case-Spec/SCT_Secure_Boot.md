**UEFI SCT II Case Specification**

# 3 Boot Services Test
## 3.4 SecureBoot Boot Services Test

**Reference Document:**

_UEFI Specification_, SecureBoot Runtime Services Section.

**Secure Boot Boot Services Functions**
| **Name** | **Type** | **Description** |
| --- | --- | --- |
| ImageLoading | Runtime | Verifies Secure Boot Image Loading. |

The Secure Boot tests require a set of keys generated for PK, KEK, db, dbx.

### 4.5.3 ImageLoading()

The ImageLoading() test uses the following images:
- TestImage1: Image that is not signed
- TestImage2: Image that is signed with Image2Cert (cert not in db)
- TestImage3: Image that is signed with Image3Cert (cert in db)
- TestImage4: Image that is signed with Image4Cert (cert in db (2nd on list))
- TestImage5: Image that is not signed (hash in db)
- TestImage6: Image that is signed with Image6Cert (cert in db, revoked in dbx with SHA256 of cert)
- TestImage7: Image that is signed with Image7Cert (cert in db, revoked in dbx with SHA384 of cert)
- TestImage8: Image that is signed with Image8Cert (cert in db, revoked in dbx with SHA512 of cert)
- TestImage9: Image that is signed with Image9Cert (cert in db, revoked in dbx with cert)
- TestImage10: Image that is signed with Image10Cert (cert in db, revoked in dbx with hash of image)
- TestImage11: Image that is signed with Image11Cert (cert in db, but image has been tampered with)
- TestImage12: Image that is signed with Image12Cert (leaf cert) with root cert (Image12Cert-root) in db
- TestImage13: Image that is signed with Image13Cert (leaf cert) with root cert (Image13Cert-root) in db and revoked in dbx with SHA256 of root cert
- TestImage14: Image that is signed with Image14Cert (leaf cert) with root cert (Image14Cert-root) in db and revoked in dbx with SHA384 of root cert
- TestImage15: Image that is signed with Image15Cert (leaf cert) with root cert (Image15Cert-root) in db and revoked in dbx with SHA512 of root cert
- TestImage16: Image that is signed with Image16Cert (leaf cert) with root cert (Image16Cert-root) in db and revoked in dbx with root cert
- TestImage17: Image that is signed with Image17Cert (leaf cert) with root cert (Image17Cert-root) in db and revoked in dbx with SHA256 of leaf cert
- TestImage18: Image that is signed with Image18Cert, with hash of image in db, and revoked in dbx with SHA256 of cert
- TestImage19: Image that is signed with Image19ACert and Image19BCert, test with each certificate in db
- TestImage20: Image that is signed with Image20ACert and Image20BCert, with Image20ACert in db and revoked in dbx with SHA256 of Image20BCert

The Image[n]Cert certificates used in this test are all created using: X.509, RSA2048, SHA256.

The variable db must be populated with a signature list that contains the following:
- Image3Cert
- Image4Cert
- Image6Cert
- Image7Cert
- Image8Cert
- Image9Cert
- Image10Cert
- Image11Cert
- Image12Cert-root
- Image13Cert-root
- Image14Cert-root
- Image15Cert-root
- Image16Cert-root
- Image17Cert-root
- EFI\_CERT\_SHA256\_GUID(TestImage5)
- EFI\_CERT\_SHA256\_GUID(TestImage18)
- Image19ACert
- Image19BCert
- Image20ACert

The variable dbx must be populated with a signature list that contains the following:
- EFI\_CERT\_X509\_SHA256\_GUID(Image6Cert)
- EFI\_CERT\_X509\_SHA384\_GUID(Image7Cert)
- EFI\_CERT\_X509\_SHA512\_GUID(Image8Cert)
- ImageCert9
- EFI\_CERT\_SHA256\_GUID(TestImage10)
- EFI\_CERT\_X509\_SHA256\_GUID(Image13Cert-root)
- EFI\_CERT\_X509\_SHA384\_GUID(Image14Cert-root)
- EFI\_CERT\_X509\_SHA512\_GUID(Image15Cert-root)
- Image16Cert-root
- EFI\_CERT\_X509\_SHA256\_GUID(Image17Cert)
- EFI\_CERT\_X509\_SHA256\_GUID(Image18Cert)
- EFI\_CERT\_X509\_SHA256\_GUID(Image20BCert)

The test infrastructure must transition the firmware to User Mode with SecureBoot=1 to run these test cases.  This must be done with the follwing steps:
- Verify SetupMode=1
- Use SetVariable() to initialize PK,KEK,db,dbx to test certificates
- Verify SetupMode=0
- Reboot the system
- Verify SetupMode=0, SecureBoot=1 (User Mode)

After the tests are complete the test infrastructure must
- Clear PK to return to Setup Mode, clear KEK,db,dbx

| **Number** | **GUID** | **Assertion** | **Test Description** |
| --- | --- | --- | --- |
| 4.5.3.1 | 0x6c605b08, 0x2ab7, 0x4681, 0x9f, 0x2f, 0x74, 0xd0, 0x8d, 0x96, 0x4e, 0x35 | **SecureBoot** - Verify load of unsigned image. | Invoke LoadImage on TestImage1 with no signature <ol type="a"><li>Verify LoadImage returns EFI\_SECURITY\_VIOLATION.</ol> |
| 4.5.3.2 | 0x34daa12d, 0x302b, 0x4afd, 0x9f, 0x1f, 0xcb, 0x55, 0x2e, 0x59, 0x48, 0xbe | **SecureBoot** - Verify load of signed image, but no cert in db. | Invoke LoadImage on TestImage2 with signature matching no certificate in db.<ol type="a"><li>Verify LoadImage returns EFI\_SECURITY\_VIOLATION.</ol> |
| 4.5.3.3 | 0x5e1c06cb, 0xf84a, 0x4b38, 0x80, 0x96, 0x2f, 0x7b, 0x95, 0x91, 0xbb, 0x3a | **SecureBoot** - Verify load of signed image, first certificate in db. | Invoke LoadImage on TestImage3 signed with first certificate in db.<ol type="a"><li>Verify LoadImage returns EFI\_SUCCESS</ol> |
| 4.5.3.4 | 0x85f7e35e, 0xe333, 0x4d44, 0x83, 0xf8, 0x03, 0x39, 0x41, 0x89, 0xf3, 0xb2 | **SecureBoot** - Verify load of signed image, second certificate in db. | Invoke LoadImage on TestImage4 signed with second certificate in db.  <ol type="a"><li>Verify LoadImage returns EFI\_SUCCESS. |
| 4.5.3.5 | 0xaa8167ae, 0xf03a, 0x4074, 0xb2, 0x78, 0xd6, 0xf1, 0x29, 0x0a, 0x86, 0xa7 | **SecureBoot** - Verify load of unsigned image with hash in db. | Invoke LoadImage on TestImage5, which is unsigned with SHA256 hash of image in db.<ol type="a"><li>Verify LoadImage returns EFI\_SUCCESS</ol> |
| 4.5.3.6 | 0xb62a25f4, 0xc24e, 0x4853, 0x9a, 0xd1, 0x8b, 0x3f, 0xc5, 0x9b, 0x29, 0xd7 | **SecureBoot** - Verify load of signed image with certificate in db and revoked in dbx with SHA256 hash. | Invoke LoadImage on TestImage6 with signature in db but revoked in dbx with EFI\_CERT\_X509\_SHA256\_GUID.<ol type="a"><li>Verify LoadImage returns EFI\_SECURITY\_VIOLATION.</ol>  |
| 4.5.3.7 | 0xf3fac69f, 0xe843, 0x431f, 0x92, 0x76, 0x36, 0x11, 0x05, 0xfd, 0x2e, 0xed | **SecureBoot** - Verify load of signed image with certificate in db and revoked in dbx with SHA384 hash. | Invoke LoadImage on TestImage7 with signature in db but revoked in dbx with EFI\_CERT\_X509\_SHA384\_GUID.<ol type="a"><li>Verify LoadImage returns EFI\_SECURITY\_VIOLATION.</ol>  |
| 4.5.3.8 | 0x71ba975a, 0x019a, 0x4a80, 0xa1, 0x13, 0x83, 0x73, 0x96, 0x31, 0x86, 0xb6 | **SecureBoot** - Verify load of signed image with certificate in db and revoked in dbx with SHA256 hash. | Invoke LoadImage on TestImage8 with signature in db but revoked in dbx with EFI\_CERT\_X509\_SHA512\_GUID.<ol type="a"><li>Verify LoadImage returns EFI\_SECURITY\_VIOLATION.</ol>  |
| 4.5.3.9 | 0x54e33072, 0x43d2, 0x4fbf, 0xb4, 0x4e, 0x1d, 0x1c, 0xe7, 0xf7, 0x1a, 0xd2 | **SecureBoot** - Verify load of signed image with certificate in db and revoked in dbx with certificate. | Invoke LoadImage on TestImage9 with signature in db, but also revoked in dbx with ImageCert9.<ol type="a"><li>Verify LoadImage returns EFI\_SECURITY\_VIOLATION.</ol>  |
| 4.5.3.10 | 0x20a23f21, 0x31f1, 0x4264, 0xa6, 0xcb, 0x7d, 0xc7, 0xe6, 0x96, 0x3c, 0x63 | **SecureBoot** - Verify load of signed image with certificate in db and revoked in dbx with hash of image. | Invoke LoadImage on TestImage10 with cert in db, but revoked with SHA256 hash of image in dbx. <ol type="a"><li>Verify LoadImage returns EFI\_SECURITY\_VIOLATION.</ol> |
| 4.5.3.11 | 0xa5851395, 0xdee0, 0x45e7, 0xa5, 0xd2, 0x8f, 0x73, 0x3e, 0x58, 0x2d, 0xc2 | **SecureBoot** - Verify load of signed image with signature that has been tampered with so signature check fails. | Invoke LoadImage on TestImage11 with certificate in db. (TestImage11 has been tampered with so signature check should fail.) <ol type="a"><li>Verify LoadImage returns EFI\_SECURITY\_VIOLATION.</ol> |
| 4.5.3.12 | 0xd8139e98, 0xf214, 0x435d, 0xb3, 0x77, 0xf9, 0xd3, 0x3e, 0xff, 0x57, 0xc4 | **SecureBoot** - Verify load of signed image, signed with leaf cert with root cert in db. | Invoke LoadImage on TestImage12, signed with leaf cert.  <ol type="a"><li>Verify LoadImage returns EFI\_SUCCESS. |
| 4.5.3.13 | 0x714197c4, 0x39c9, 0x4fba, 0xb3, 0xc3, 0x98, 0x55, 0x9c, 0x07, 0x63, 0xe8 | **SecureBoot** - Verify load of image signed with leaf cert, root cert in db, root cert revoked in dbx with SHA256 hash. | Invoke LoadImage on TestImage13 with root cert in db but revoked in dbx with EFI\_CERT\_X509\_SHA256\_GUID.<ol type="a"><li>Verify LoadImage returns EFI\_SECURITY\_VIOLATION.</ol>  |
| 4.5.3.14 | 0x90b4252a, 0x6823, 0x4701, 0xa9, 0x41, 0xe0, 0x24, 0x9f, 0xed, 0x98, 0x0c | **SecureBoot** - Verify load of image signed with leaf cert, root cert in db, root cert revoked in dbx with SHA348 hash. | Invoke LoadImage on TestImage14 with root cert in db but revoked in dbx with EFI\_CERT\_X509\_SHA384\_GUID.<ol type="a"><li>Verify LoadImage returns EFI\_SECURITY\_VIOLATION.</ol>  |
| 4.5.3.15 | 0xb35ccc23, 0x53cb, 0x4728, 0xb5, 0xa4, 0x08, 0x72, 0xe0, 0xf9, 0xce, 0x4c | **SecureBoot** - Verify load of image signed with leaf cert, root cert in db, root cert revoked in dbx with SHA512 hash. | Invoke LoadImage on TestImage15 with root cert in db but revoked in dbx with EFI\_CERT\_X509\_SHA512\_GUID.<ol type="a"><li>Verify LoadImage returns EFI\_SECURITY\_VIOLATION.</ol>  |
| 4.5.3.16 | 0x11f0c418, 0x8afc, 0x406a, 0xb0, 0x4b, 0x1f, 0x7d, 0x9f, 0xac, 0xa9, 0x6b | **SecureBoot** - Verify load of image signed with leaf cert, root cert in db, root cert revoked in dbx with root cert. | Invoke LoadImage on TestImage16 with certificate db, but also revoked in dbx with same certificate.<ol type="a"><li>Verify LoadImage returns EFI\_SECURITY\_VIOLATION.</ol>  |
| 4.5.3.17 | 0xb0c99a7a, 0x9add, 0x4d1d, 0x9d, 0x2e, 0xbd, 0x4b, 0xe2, 0xb4, 0xaa, 0xee | **SecureBoot** - Verify load of image signed with leaf cert, root cert in db, leaf cert revoked in dbx with SHA256 hash. | Invoke LoadImage on TestImage17 with root cert in db but revoked in dbx with EFI\_CERT\_X509\_SHA256\_GUID.<ol type="a"><li>Verify LoadImage returns EFI\_SECURITY\_VIOLATION.</ol>  |
| 4.5.3.18 | 0xc4373b07, 0xa84e, 0x43e9, 0x87, 0x33, 0x37, 0x7a, 0x6c, 0x59, 0x8a, 0x3b | **SecureBoot** - Verify load of signed image, with hash of image in db, and revoked in dbx with SHA256 hash of certificate. | Invoke LoadImage on TestImage18, with hash of image in db but revoked in dbx with EFI\_CERT\_X509\_SHA256\_GUID.<ol type="a"><li>Verify LoadImage returns EFI\_SECURITY\_VIOLATION.</ol>  |
| 4.5.3.19 | 0x29acbb98, 0xdabe, 0x43c1, 0x8d, 0x9c, 0x51, 0xaf, 0x51, 0xd4, 0x1f, 0x75 | **SecureBoot** - Verify load of image containing multiple signatures. | Test scenarios with each signature in db. <ol type="a"><li>Install 1st signature in db, Invoke LoadImage on TestImage19<li>Verify LoadImage returns EFI\_SUCCESS.<li>Install 2nd signature in db, Invoke LoadImage on TestImage19<li>Verify LoadImage returns EFI\_SUCCESS.</ol> |
| 4.5.3.20 | 0xe88e687d, 0xfd1c, 0x486e, 0xa4, 0x16, 0x8f, 0x10, 0x3c, 0x5c, 0xe0, 0xd0 | **SecureBoot** - Verify load of image containing multiple signatures, second cert in dbx. | Invoke LoadImage on TestImage20, signed with multiple certs, 1st cert in db, 2nd cert in dbx.  <ol type="a"><li>Verify LoadImage returns EFI\_SECURITY\_VIOLATION.</ol> |
| 4.5.3.21 | 0xdb237675, 0xff01, 0x4472, 0xa5, 0x97, 0xd8, 0xca, 0x6b, 0xb9, 0x0b, 0xdc | **SecureBoot** - Verify load of signed image, with SecureBoot=0. | Test steps:<ol type="a"><li>Verify SecureBoot==0<li>Invoke LoadImage on TestImage2.<li>Verify LoadImage returns EFI\_SUCCESS.</ol> |

<br>
The following tests examine the Image Execution Info Table based on tests 4.5.3.1 - 4.5.3.21

| **Number** | **GUID** | **Assertion** | **Test Description** |
| --- | --- | --- | --- |
| 4.5.3.22 | 0x4bb36713, 0xd8e2, 0x405b, 0x8b, 0xda, 0x86, 0x12, 0xc7, 0x92, 0x91, 0xa9 | **SecureBoot** - Verify Image Execution Info Table entry for TestImage1, image not signed. | Read the Image Execution Information Table (EFI\_IMAGE\_EXECUTION\_INFO\_TABLE).<ol type="a"><li>Verify that image name is TestImage1<li>Verify that action is UNTESTED.</ol> |
| 4.5.3.23 | 0x3b48371b, 0xc27e, 0x4c19, 0xb9, 0x99, 0x54, 0x53, 0x7e, 0xa4, 0xbf, 0xed | **SecureBoot** - Verify Image Execution Info Table entry for TestImage2, image signed but cert not in db. | Read the Image Execution Information Table (EFI\_IMAGE\_EXECUTION\_INFO\_TABLE).<ol type="a"><li>Verify that image name is TestImage2<li>Verify that action is SIG\_NOT\_FOUND.</ol>  |
| 4.5.3.24 | 0xb3a670aa, 0x0fba, 0x48ca, 0x9d, 0x01, 0x0e, 0xe9, 0x70, 0x09, 0x65, 0xa9 | **SecureBoot** - Verify Image Execution Info Table entry for TestImage6, revoked by hash of cert | Read the Image Execution Information Table (EFI\_IMAGE\_EXECUTION\_INFO\_TABLE).<ol type="a"><li>Verify that image name is TestImage6<li>Verify that action is SIG\_FAILED.</ol> |
| 4.5.3.25 | 0x00c3c2f2, 0x39d5, 0x4d35, 0xb7, 0xe7, 0x58, 0x7c, 0xa0, 0xf3, 0xcb, 0x75 | **SecureBoot** - Verify Image Execution Info Table entry for TestImage7, revoked by hash of cert. | Read the Image Execution Information Table (EFI\_IMAGE\_EXECUTION\_INFO\_TABLE).<ol type="a"><li>Verify that image name is TestImage7<li>Verify that action is SIG\_FAILED.</ol> |
| 4.5.3.26 | 0x55211c3f, 0xc438, 0x485a, 0xb1, 0x75, 0xb1, 0xef, 0x5d, 0x31, 0x41, 0xc7 | **SecureBoot** - Verify Image Execution Info Table entry for TestImage8, revoked by hash of cert. | Read the Image Execution Information Table (EFI\_IMAGE\_EXECUTION\_INFO\_TABLE).<ol type="a"><li>Verify that image name is TestImage8<li>Verify that action is SIG\_FAILED.</ol> |
| 4.5.3.27 | 0x32a28ae3, 0x9dcb, 0x4f3e, 0x86, 0x24, 0xe3, 0x3e, 0x50, 0xa9, 0x33, 0xe1 | **SecureBoot** - Verify Image Execution Info Table entry for TestImage9, revoked by cert.  | Read the Image Execution Information Table (EFI\_IMAGE\_EXECUTION\_INFO\_TABLE).<ol type="a"><li>Verify that image name is TestImage9<li>Verify that action is SIG\_FAILED.</ol> |
| 4.5.3.28 | 0x9031ee38, 0x3e1a, 0x44f2, 0x8e, 0x08, 0xf5, 0x2b, 0x52, 0xad, 0x1b, 0xd9 | **SecureBoot** - Verify Image Execution Info Table entry for TestImage10, revoked by hash of image. | Read the Image Execution Information Table (EFI\_IMAGE\_EXECUTION\_INFO\_TABLE).<ol type="a"><li>Verify that image name is TestImage10<li>Verify that action is SIG\_FOUND.</ol> |
| 4.5.3.29 | 0x09416a34, 0x3235, 0x4efb, 0xba, 0x60, 0x9a, 0x83, 0x4c, 0xd3, 0xe3, 0x2f  | **SecureBoot** - Verify Image Execution Info Table entry for TestImage11, signed image that has been tampered with. | Read the Image Execution Information Table (EFI\_IMAGE\_EXECUTION\_INFO\_TABLE).<ol type="a"><li>Verify that image name is TestImage11<li>Verify that action is SIG\_FAILED.</ol> |
| 4.5.3.30 | 0xa89b6422, 0xbf83, 0x4903, 0x87, 0xcb, 0x88, 0x57, 0x30, 0x92, 0x98, 0x69 | **SecureBoot** - Verify Image Execution Info Table entry for TestImage13, revoked by hash of cert | Read the Image Execution Information Table (EFI\_IMAGE\_EXECUTION\_INFO\_TABLE).<ol type="a"><li>Verify that image name is TestImage13<li>Verify that action is SIG\_FAILED.</ol> |
| 4.5.3.31 | 0x91df3323, 0xb7fd, 0x4b1f, 0xa4, 0x87, 0xfb, 0x1e, 0xc1, 0xa1, 0xcf, 0xb7 | **SecureBoot** - Verify Image Execution Info Table entry for TestImage14, revoked by hash of cert | Read the Image Execution Information Table (EFI\_IMAGE\_EXECUTION\_INFO\_TABLE).<ol type="a"><li>Verify that image name is TestImage14<li>Verify that action is SIG\_FAILED.</ol> |
| 4.5.3.32 | 0xdf81cd53, 0xba80, 0x4657, 0x87, 0xbd, 0xbe, 0xf5, 0xfc, 0x73, 0x24, 0x07 | **SecureBoot** - Verify Image Execution Info Table entry for TestImage15, revoked by hash of cert | Read the Image Execution Information Table (EFI\_IMAGE\_EXECUTION\_INFO\_TABLE).<ol type="a"><li>Verify that image name is TestImage15<li>Verify that action is SIG\_FAILED.</ol> |
| 4.5.3.33 | 0x83fc201b, 0x43c8, 0x4c9f, 0xac, 0x48, 0x47, 0x66, 0x34, 0x81, 0x80, 0x36 | **SecureBoot** - Verify Image Execution Info Table entry for TestImage16, revoked by cert | Read the Image Execution Information Table (EFI\_IMAGE\_EXECUTION\_INFO\_TABLE).<ol type="a"><li>Verify that image name is TestImage16<li>Verify that action is SIG\_FAILED.</ol> |
| 4.5.3.34 | 0xa13cf711, 0x3bd6, 0x464d, 0x9c, 0x97, 0x38, 0x40, 0xd0, 0xcc, 0xb8, 0xad | **SecureBoot** - Verify Image Execution Info Table entry for TestImage17, revoked by hash of cert | Read the Image Execution Information Table (EFI\_IMAGE\_EXECUTION\_INFO\_TABLE).<ol type="a"><li>Verify that image name is TestImage17<li>Verify that action is SIG\_FAILED.</ol> |
| 4.5.3.35 | 0xc377fcbc, 0xad2a, 0x45dc, 0xbe, 0xfb, 0x17, 0xe8, 0xe5, 0xcb, 0x1f, 0x15 | **SecureBoot** - Verify Image Execution Info Table entry for TestImage18, revoked by hash of cert | Read the Image Execution Information Table (EFI\_IMAGE\_EXECUTION\_INFO\_TABLE).<ol type="a"><li>Verify that image name is TestImage18<li>Verify that action is SIG\_FAILED.</ol> |
| 4.5.3.36 | 0x523e5c4b, 0x789b, 0x4283, 0xb9, 0x5e, 0x04, 0x1f, 0x28, 0x70, 0x3a, 0x30 | **SecureBoot** - Verify Image Execution Info Table entry for TestImage20, revoked by hash of cert | Read the Image Execution Information Table (EFI\_IMAGE\_EXECUTION\_INFO\_TABLE).<ol type="a"><li>Verify that image name is TestImage20<li>Verify that action is SIG\_FAILED.</ol> |


# 4 Services Runtime Services Test

## 4.5 Secure Boot Runtime Services Test

**Reference Document:**

_UEFI Specification_, SecureBoot Runtime Services Section.

**SecureBoot Runtime Services Functions**
| **Name** | **Type** | **Description** |
| --- | --- | --- |
| SecureBootStates | Runtime | Verifies Secure Boot state transitions. |
| VariableUpdate | Runtime | Verifies updates to Secure Boot variables. |

The Secure Boot tests require a set of keys generated for PK, KEK, db, dbx.

### 4.5.1 SecureBootState()

This test verifies the secure boot state transitions defined in the UEFI specification that can be tested without platform specific mechanisms:

![state-transitions](images/sbstates.png)

This test assumes the system is in SetupMode.


| **Number** | **GUID** | **Assertion** | **Test Description** |
| --- | --- | --- | --- |
| 4.5.1.1 | 0xc35a351c, 0x053f, 0x4531, 0xb7, 0xde, 0x26, 0x33, 0xd1, 0x4e, 0x07, 0x02 | **SecureBoot** - Transition from Setup Mode to User mode. | Enroll PK using SetVariable(). Verify:<ol type="a"><li>AuditMode=0<li>DeployedMode=0<li>SetupMode=0<li>SecureBoot=0<li>Attributes of AuditMode,SetupMode,DeployedMode,SecureBoot are BS,RT<li>SetupMode is read-only</ol> |
| 4.5.1.2 | 0x3250ca46, 0x4b36, 0x4478, 0x90, 0x9f, 0x44, 0x84, 0xf8, 0x67, 0x37, 0xd1 | **SecureBoot** - Transition from User Mode to Setup Mode. | Clear PK using SetVariable(). Verify:<ol type="a"><li>AuditMode=0<li> DeployedMode=0<li> SetupMode=1<li> SecureBoot=0<li>Attributes of AuditMode,SetupMode,DeployedMode,SecureBoot are BS,RT<li>SetupMode is read-only<li>DeployedMode is read-only</ol> |
| 4.5.1.3 | 0x3741bfab, 0x8991, 0x4376, 0xa8, 0x1f, 0xed, 0x13, 0x64, 0xe6, 0x43, 0x92 | **SecureBoot** - Transition from Setup Mode to User Mode. | Enroll PK using SetVariable(). Verify:<ol type="a"><li>AuditMode=0<li> DeployedMode=0<li> SetupMode=0<li> SecureBoot=0<li>Attributes of AuditMode,SetupMode,DeployedMode,SecureBoot are BS,RT</ol> |
| 4.5.1.4 | 0x48b9af5b, 0xda19, 0x4cbf, 0x9d, 0xff, 0xf0, 0x8c, 0xe6, 0xb7, 0x8d, 0x1d | **SecureBoot** - Transition from User Mode to Deployed Mode. | Set DeployedMode=1 using SetVariable(). Verify:<ol type="a"><li>AuditMode=0<li> DeployedMode=1<li> SetupMode=0<li> SecureBoot=0<li>Attributes of AuditMode,SetupMode,DeployedMode,SecureBoot are BS,RT<li>SetupMode is read-only<li>DeployedMode is read-only<li>AuditMode is read-only</ol> |
| 4.5.1.5 | 0x5f66a939, 0x4466, 0x4942, 0xa1, 0xa7, 0xff, 0xa8, 0xb9, 0xa5, 0x35, 0xff | **SecureBoot** - Transition from Deployed Mode to Setup Mode. | Clear PK using SetVariable(). Verify:<ol type="a"><li>AuditMode=0<li> DeployedMode=0<li> SetupMode=1<li> SecureBoot=0<li>Attributes of AuditMode,SetupMode,DeployedMode,SecureBoot are BS,RT</ol> |
| 4.5.1.6 | 0xe4357130, 0xd4c9, 0x49b6, 0x8b, 0x22, 0x7f, 0xff, 0x9b, 0xe2, 0xf9, 0x91 | **SecureBoot** - Transition from Setup Mode to User Mode. | Enroll PK using SetVariable(). Verify:<ol type="a"><li>AuditMode=0<li> DeployedMode=0<li> SetupMode=0<li> SecureBoot=0<li>Attributes of AuditMode,SetupMode,DeployedMode,SecureBoot are BS,RT</ol> |
| 4.5.1.7 | 0xb218db38, 0x122c, 0x4d90, 0x89, 0xd0, 0x1a, 0x6b, 0xfe, 0x88, 0x38, 0xeb | **SecureBoot** - Transition from User Mode to Audit Mode. | Set AuditMode=1 using SetVariable(). Verify:<ol type="a"><li>PK is clear<li> AuditMode=1<li> DeployedMode=0<li> SetupMode=1<li> SecureBoot=0<li>Attributes of AuditMode,SetupMode,DeployedMode,SecureBoot are BS,RT<li>SetupMode is read-only<li>DeployedMode is read-only<li>AuditMode is read-only</ol> |
| 4.5.1.8 | 0x267b0177, 0xa24a, 0x427f, 0xa1, 0xab, 0x0e, 0x49, 0x4a, 0x4e, 0x26, 0x36 | **SecureBoot** - Transition from Audit Mode to Deployed Mode. | Enroll PK using SetVariable(). Verify:<ol type="a"><li>AuditMode=0<li> DeployedMode=1<li> SetupMode=0<li> SecureBoot=0<li>Attributes of AuditMode,SetupMode,DeployedMode,SecureBoot are BS,RT</ol> |
| 4.5.1.9 | 0x0ab53752, 0xdd97, 0x4289, 0xb6, 0x19, 0xd7, 0xbd, 0x8a, 0xc8, 0x6e, 0x78 | **SecureBoot** - Transition from Deployed Mode to Setup Mode. | Clear PK using SetVariable(). Verify:<ol type="a"><li>AuditMode=0<li> DeployedMode=0<li> SetupMode=1<li> SecureBoot=0<li>Attributes of AuditMode,SetupMode,DeployedMode,SecureBoot are BS,RT</ol> |
| 4.5.1.10 | 0x566d8c07, 0x6c13, 0x41dd, 0x8f, 0xad, 0xfc, 0xf5, 0x0a, 0x91, 0xc7, 0xf5 | **SecureBoot** - Transition from Setup Mode to Audit Mode.  | Set AuditMode=1 using SetVariable(). Verify:<ol type="a"><li>PK is clear<li> AuditMode=1<li> DeployedMode=0<li> SetupMode=1<li> SecureBoot=0<li>Attributes of AuditMode,SetupMode,DeployedMode,SecureBoot are BS,RT</ol> |
| 4.5.1.11 | 0xc57ab7e0, 0x78c0, 0x448b, 0x94, 0x2e, 0xff, 0xfb, 0x12, 0x37, 0x48, 0x03 | **SecureBoot** - Transition from Audit Mode to Deployed Mode. | Enroll PK using SetVariable(). Verify:<ol type="a"><li>AuditMode=0<li> DeployedMode=1<li> SetupMode=0<li> SecureBoot=0<li>Attributes of AuditMode,SetupMode,DeployedMode,SecureBoot are BS,RT</ol> |
| 4.5.1.12 | 0x068ed119, 0x401e, 0x46a0, 0x9c, 0x56, 0xd5, 0x65, 0x3a, 0xb3, 0x7b, 0x3d | **SecureBoot** - Transition from Deployed Mode to Setup Mode. | Clear PK using SetVariable(). Verify:<ol type="a"><li>AuditMode=0<li> DeployedMode=0<li> SetupMode=1<li> SecureBoot=0<li>Attributes of AuditMode,SetupMode,DeployedMode,SecureBoot are BS,RT</ol> |

### 4.5.2 VariableUpdates()

The test infrastructure must transition the firmware to User Mode (with PK enrolled):
- Verify SetupMode=1
- Use SetVariable() to initialize PK to a test certificate
- Verify SetupMode=0

After the tests are complete the test infrastructure must
- Clear PK to return to Setup Mode, clear KEK,db,dbx

| **Number** | **GUID** | **Assertion** | **Test Description** |
| --- | --- | --- | --- |
| 4.5.2.1 | 0x68054c5f, 0x011e, 0x42c5, 0x9f, 0x9d, 0x23, 0xc4, 0xba, 0x0e, 0x37, 0x42 | **SecureBoot** - Verify create operation on secure boot variables, authorized by PK. | For each variable (KEK,db,dbx), use SetVariable to set to data (authorized by PK)<ol type="a"><li>Verify that function returns EFI\_SUCCESS.</ol> |
| 4.5.2.2 | 0x743ca7ad, 0x8dc3, 0x4d29, 0x80, 0xed, 0xd2, 0x69, 0xcc, 0x9b, 0x1d, 0x8d | **SecureBoot** - Verify the attributes for PK and KEK are as defined by 3.3 Globally Defined Variables in the UEFI spec. | For each variable (PK,KEK), use GetVariable to get the variable attributes<ol type="a"><li>Verify the variable attributes for PK/KEK.  Attributes should be NV,BS,RT,AT.</ol> |
| 4.5.2.3 | 0x3dd97680, 0xfd18, 0x4b33, 0xaa, 0x9c, 0xdb, 0xf0, 0x81, 0x60, 0xfb, 0x57 | **SecureBoot** - Verify delete operation on secure boot variables, authorized by PK. | For each variable (KEK,db,dbx), use SetVariable to set to null (authorized by PK)<ol type="a"><li>Verify that function returns EFI\_SUCCESS.<li>Verify that GetVariable() returns EFI\_NOT\_FOUND.</ol> |
| 4.5.2.4 | 0xf6778756, 0xf6f8, 0x42fe, 0x8e, 0x66, 0x0c, 0x28, 0x95, 0x71, 0x77, 0x45 | **SecureBoot** - Verify delete operation on secure boot variables with mismatched attributes. | For each variable (KEK,db,dbx), use SetVariable to set to null (authorized by PK), but with attributes that don't match existing ones.<ol type="a"><li>Verify that function returns EFI\_INVALID\_PARAMETER.</ol> |
| 4.5.2.5 | 0x97bc750e, 0x84c9, 0x4416, 0x9e, 0x27, 0x49, 0xc8, 0x66, 0x2c, 0xef, 0xb6 | **SecureBoot** - Verify signed update of secure boot variables with unauthorized signer. | For each variable (KEK,db,dbx), use SetVariable to set to data<ol type="a"><li>Verify that function returns EFI\_SECURITY\_VIOLATION.</ol> |
| 4.5.2.6 | 0xe280f1b0, 0x0d4c, 0x42d4, 0x9a, 0xc3, 0xa1, 0xff, 0xc6, 0xaa, 0xba, 0xba | **SecureBoot** - Test update of secure boot variables with unsigned data. | For each variable (KEK,db,dbx), use SetVariable with unsigned data.<ol type="a"><li>Verify that function returns EFI\_SECURITY\_VIOLATION.</ol> |
| 4.5.2.7 | 0x6c63399e, 0xf8ab, 0x4257, 0xa0, 0x6e, 0x69, 0x10, 0x92, 0xaf, 0x69, 0x94 | **SecureBoot** - Test update of secure boot variables with non-zero Pad1, Nanosecond, TimeZone, Daylight and Pad2 fields in the timestamp. | For db use SetVariable with signed data with non-zero Pad1, Nanosecond, TimeZone, Daylight and Pad2 fields in the timestamp.<ol type="a"><li>Verify that function returns EFI\_SECURITY\_VIOLATION.</ol> |
| 4.5.2.8 | 0x1cb15937, 0x3916, 0x4dac, 0x87, 0xc4, 0x26, 0xd2, 0xc8, 0xa4, 0x4b, 0x65 | **SecureBoot** - Test update of secure boot variables that already exist with data authorized by PK. | For each variable (KEK,db,dbx), use SetVariable with data authorized by PK.<ol type="a"><li>Verify that function returns EFI\_SUCCESS.</ol> |
| 4.5.2.9 | 0xcc1d50fa, 0xa46c, 0x4a61, 0x8a, 0x98, 0x5e, 0x25, 0xb5, 0x2b, 0x72, 0x05 | **SecureBoot** - Test update of db,dbx with data authorized by KEK. | For each variable (db,dbx), use SetVariable with data authorized by KEK.<ol type="a"><li>Verify that function returns EFI\_SUCCESS.</ol> |
| 4.5.2.10 | 0x00158dd8, 0xcfca, 0x42fc, 0x85, 0x19, 0xa9, 0x20, 0x4e, 0x19, 0x36, 0x3c | **SecureBoot** - Verify update of db,dbx wth data authorized by second signature in KEK signature list. | Invoke SetVariable to set db to signature list authorized by second signature in KEK signature list.<ol type="a"><li>Verify that function returns EFI\_SUCCESS.</ol> |
| 4.5.2.11 | 0xab969f35, 0xfe3e, 0x4408, 0xad, 0xd8, 0xe0, 0xe3, 0xe4, 0x83, 0x2c, 0x74 | **SecureBoot** - Verify append to KEK with data authorized by PK. | Invoke SetVariable to append signature list to KEK.<ol type="a"><li>Verify that function returns EFI\_SUCCESS.</ol>|
| 4.5.2.12 | 0x16f5a492, 0x3946, 0x4514, 0xba, 0xe5, 0x0b, 0xa1, 0xf0, 0x83, 0xcb, 0xcc | **SecureBoot** - Verify append to db,dbx with data authorized by signature appended to KEK in 4.5.2.9. | Invoke SetVariable to append a db,dbx signature list authorized by the signature appended to KEK in 4.5.2.9.<ol type="a"><li>Verify that function returns EFI\_SUCCESS.</ol>|
| 4.5.2.13 | 0x0729b175, 0x5975, 0x4849, 0xab, 0x50, 0x92, 0x85, 0xf1, 0xef, 0xcf, 0x61 | **SecureBoot** - Attempt to perform unsigned delete on secure boot variables. | For each variable (PK,KEK,db,dbx), use SetVariable with unsigned null data to do a delete.<ol type="a"><li>Verify that function returns EFI\_SECURITY\_VIOLATION.</ol> |
