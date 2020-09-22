<div align="center">

## Mac Address Via Netbios


</div>

### Description

Unlike the complex code required to obtain your own MAC address, this is considerably easier involving three API calls in total.

The iphlpapi.dll library exposes a SendArp call whose only purpose is send an ARP request to obtain the physical address that corresponds to the specified destination IP address.
 
### More Info
 
The DestIP member specifies the destination IP address, and the ARP request attempts to obtain the physical address corresponding to this IP address.

SrcIP can specify the IP address of the sender. This is optional, and zero for this parameter.

Following the call the pMacAddr member contains the pointer to an array of ULONG variables. The first six bytes of this array receive the physical address that corresponds to the IP address specified by the DestIP parameter. These are copied to a byte array, from which the MAC address is extracted.

PhyAddrLen is passed initially with &HFF (255), and on return contains the length of the physical address pointed to by the pMacAddr parameter (which should be 6). .

Note that this call is only available under Windows 2000; Windows XP Pro; and Windows .NET Server. No corresponding NT4 or 9x call is supplied.

Note: One reader has informed me that while SendArp works on determining the MAC address of another device on the same network segment, it doesn't return a MAC address for a controller that is on another network segment where it has to go through a router -- the pointer to the MAC address (pMacAddr) returns with a value of zero. This caused the application to crash on hitting the CopyMemory call, thus I have added code to the routine below to ensure CopyMemory is not called when pMacAddr or PhyAddrLen are returned as 0.

Note 2: Another user has reported a problem with the code below when compiled to an exe. I've identified the problem as a combination of Native code compiling and the assignment of a string return type to the demo's GetRemoteMACAddress function (bugs with Native compiling are not news!). To remedy this problem, the code below has been updated to change the return type of the function to Boolean from String, and to pass an empty string to the function for it to fill with the remote MAC address. These changes mean the code to set the error message (the msgboxes) must be moved out of the main routine into the calling routine. With the modified code below compiling to either pcode or native code will work.

Note 3: In earlier versions of this code, I had previously stated that passing the local machine's IP address crashed VB. While true with the old version, an astute reader identified the problem as pre-allocating the value &HFF (255) to PhyAddrLen. His tests, verified here and by further examination of the MSDN, has confirmed that the value assigned to PhyAddrLen should be 6. The code below, which reflects this change, can now be used to successfully return both a remote or local machine's MAC address without issue.


<span>             |<span>
---                |---
**Submitted On**   |2004-06-11 06:27:02
**By**             |[Celentano](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/celentano.md)
**Level**          |Intermediate
**User Rating**    |4.8 (24 globes from 5 users)
**Compatibility**  |VB 5\.0, VB 6\.0
**Category**       |[Internet/ HTML](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/internet-html__1-34.md)
**World**          |[Visual Basic](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/visual-basic.md)
**Archive File**   |[Mac\_Addres1756046112004\.zip](https://github.com/Planet-Source-Code/celentano-mac-address-via-netbios__1-54310/archive/master.zip)








