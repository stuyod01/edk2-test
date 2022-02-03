![Shape1](RackMultipart20220203-4-14se6jk_html_1248b441cef54492.gif)

# **Self Certification Test (SCT) II
 Case Specification**

**Version 2.6 Errata A**

**June 2017**

The material contained herein is not a license, either expressly or impliedly, to any intellectual property owned or controlled by any of the authors or developers of this material or to any contribution thereto. The material contained herein is provided on an &quot;AS IS&quot; basis and, to the maximum extent permitted by applicable law, this information is provided AS IS AND WITH ALL FAULTS, and the authors and developers of this material hereby disclaim all other warranties and conditions, either express, implied or statutory, including, but not limited to, any (if any) implied warranties, duties or conditions of merchantability, of fitness for a particular purpose, of accuracy or completeness of responses, of results, of workmanlike effort, of lack of viruses and of lack of negligence, all with regard to this material and any contribution thereto. Designers must not rely on the absence or characteristics of any features or instructions marked &quot;reserved&quot; or &quot;undefined.&quot; The Unified EFI Forum, Inc. reserves any features or instructions so marked for future definition and shall have no responsibility whatsoever for conflicts or incompatibilities arising from future changes to them. ALSO, THERE IS NO WARRANTY OR CONDITION OF TITLE, QUIET ENJOYMENT, QUIET POSSESSION, CORRESPONDENCE TO DESCRIPTION OR NON-INFRINGEMENT WITH REGARD TO THE SPECIFICATION AND ANY CONTRIBUTION THERETO.

IN NO EVENT WILL ANY AUTHOR OR DEVELOPER OF THIS MATERIAL OR ANY CONTRIBUTION THERETO BE LIABLE TO ANY OTHER PARTY FOR THE COST OF PROCURING SUBSTITUTE GOODS OR SERVICES, LOST PROFITS, LOSS OF USE, LOSS OF DATA, OR ANY INCIDENTAL, CONSEQUENTIAL, DIRECT, INDIRECT, OR SPECIAL DAMAGES WHETHER UNDER CONTRACT, TORT, WARRANTY, OR OTHERWISE, ARISING IN ANY WAY OUT OF THIS OR ANY OTHER AGREEMENT RELATING TO THIS DOCUMENT, WHETHER OR NOT SUCH PARTY HAD ADVANCE NOTICE OF THE POSSIBILITY OF SUCH DAMAGES.

Copyright 2010-2017, Unified EFI Forum, Inc. All Rights Reserved.

# **Revision History**

| **Version** | **Revision Notes** | **Release Date** |
| --- | --- | --- |
| 2.1 | Initial Release | May 2009 |
| 2.3 | Add chapter for Vlan and EAP. Additional materials to EFI\_SIMPLE\_TEXT\_OUTPUT\_PROTOCOL test, also DHCPv6, TCP6, IPv6, IP6Config, IPsecConfig, UDPv6 and MTFTPv6 | June 2010 |
| 2.3 | Mantis618 content integration643 Add test case description of Firmware Management Protocol646 ATA Pass Thru Protocol699 SetInfo and StringToImage Integration672 InstallSCT application need to support on NT32673 New feature request for Verbose function in SCT | Jan. 2011 |
| 2.3.1 C | Mantis643 Firmware Management Protocol Test Case710 Match SCT Case Specification 2.3 to UEFI spec 2.3827 Char issues in Guid format corrected and missed Index appended832 Some index numbers in spec corrected835 Guid Format corrections841 Corrected some duplicated Index/Guid843 Case Spec refreshed to include new items of UEFI 2.3.1 Spec | Oct. 2011 |
| 2.3.1 C | Mantis 939 Update to align with UEFI Spec 2.3.1 Errata C | Aug. 2012 |
| 2.4 B | Mantis 1295 Update to align with UEFI Spec 2.4 Errata B | Dec. 2014 |
| 2.5 A | Mantis 1733 Update to align with UEFI Spec 2.5 Errata A | Jan. 2017 |
| 2.6 A | Mantis 1807 Update to align with UEFI Spec 2.6 Errata A | June 2017 |