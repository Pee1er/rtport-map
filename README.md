Title: Realtek rtport.sys physical memory map

Vendor: Realtek Semiconductor Corp.
Product: Generic Port I/O
Component: rtport.sys
SHA-256: 71423a66165782efb4db7be6ce48ddb463d9f65fd0f266d333a6558791d158e5
MD5: 4ca0dba9e224473d664c25e411f5a3bd
SHA-1: c22dc62e10378191840285814838fe9ed1af55d7
Size: 15144 bytes
PE timestamp: 2005-08-01 09:36:00 UTC
FileVersion: 5.0.2195.1711
ProductVersion: 5.0.2195.1711
OriginalFilename: rtport.sys
Authenticode status: Valid
Signer: CN=Realtek Semiconductor Corp, OU=RTCN, OU=Digital ID Class 3 - Microsoft Software Validation v2, O=Realtek Semiconductor Corp, L=Hsinchu, S=Taiwan, C=TW
Certificate thumbprint: 5EC2BC0BF95A146D5B39F23906B66DF039056E8E
Certificate not before: 2006-05-20 00:00:00 UTC
Certificate not after: 2007-06-12 23:59:59 UTC
Device: \\.\rtport
PDB: D:\WDM\Work\RTIO\objfre_wnet_AMD64\amd64\rtport.pdb

CVE:

Description:
rtport.sys in Realtek Semiconductor Corp. Generic Port I/O FileVersion 5.0.2195.1711 (SHA-256 71423a66165782efb4db7be6ce48ddb463d9f65fd0f266d333a6558791d158e5, 15144 bytes, PE timestamp 2005-08-01 09:36:00 UTC) passes a caller-supplied 64-bit physical address to MmMapIoSpace on IOCTL 0x9C72680C (METHOD_BUFFERED), keeps the high 32 bits of that address, maps a fixed length of 0x200 bytes as non-cached, does not validate the address range, and returns the kernel virtual address of that mapping, which allows an administrator to write one byte and read one byte of that physical memory via IOCTL 0x9C72A840 and IOCTL 0x9C726800 on \\.\rtport. VERSIONINFO on this file is the Windows 2003 DDK template: CompanyName and ProductName are Windows (R) 2003 DDK 3790 provider, FileDescription is Generic Port I/O for Win64, FileVersion and ProductVersion are 5.0.2195.1711, and OriginalFilename is rtport.sys. The Authenticode status is Valid, signed by CN=Realtek Semiconductor Corp, OU=RTCN, OU=Digital ID Class 3 - Microsoft Software Validation v2, O=Realtek Semiconductor Corp, L=Hsinchu, S=Taiwan, C=TW, thumbprint 5EC2BC0BF95A146D5B39F23906B66DF039056E8E. On 2026-11-03, on an isolated Windows 10 Enterprise LTSC 2021 virtual machine (10.0.19044.7807), an administrator loaded this file and the service started. That administrator locked one of its own pages at physical address 0x1aed9000, opened \\.\rtport, and sent IOCTL 0x9C72680C for that address. Through IOCTL 0x9C72A840 that process wrote 0x5A at the returned kernel address and read 0x5A back through IOCTL 0x9C726800, then wrote 0xA5 and read 0xA5 back the same way. The same process read the first byte of that physical page as 0x5A, then 0xA5. The test did not show a crash. The device security descriptor was O:BAG:SYD:(A;;0x1201bf;;;WD)(A;;FA;;;SY)(A;;FA;;;BA)(A;;0x1200a9;;;RC)S:AI(ML;;NW;;;LW). Everyone (WD) has access mask 0x1201bf, which includes FILE_READ_DATA and FILE_WRITE_DATA.

Suggested fix: on IOCTL 0x9C72680C, IOCTL 0x9C72A840, and IOCTL 0x9C726800, use only a physical address chosen inside the driver, and limit access to \\.\rtport to Administrators and SYSTEM.
