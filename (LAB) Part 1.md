# Lab 1-1
This lab uses the files Lab01-01.exe and Lab01-01.dll. Use the tools and techniques described in the chapter to gain information about the files and answer the questions below.
## Questions
1. Upload the files to http://www.VirusTotal.com/ and view the reports. Does either file match any existing antivirus signatures?

**Lab01-01.exe:** Yes (sha256: 58898bd42c5bd3bf9b1389f0eee5b39cd59180e8370eb9ea838a0b327bd6fe47)

**Lab01-01.dll:** Yes (sha256: f50e42c8dfaab649bde0398867e930b86c2a599e8db83b8260393082268f2dba)

2. When were these files compiled?
*Open the files in PEview (In my case, I used pestudio and it still worked fine)*

**Lab01-01.exe:** Sun Dec 19 16:16:19 2010 (UTC)

**Lab01-01.dll:** Sun Dec 19 16:16:38 2010 (UTC)

3. Are there any indications that either of these files is packed or obfuscated? If so, what are these indicators?
*Open the files with PEiD*

**Lab01-01.exe:** PEiD didn't detect a packer

**Lab01-01.dll:** PEiD didn't detect a packer

4. Do any imports hint at what this malware does? If so, which imports are they?
*Use Dependency Walker to check for imports. Dependency Walker was giving me some errors so I used the newer Dependencies program instead*

*In the program, double click the file you loaded on the top left corner to see its imports*

**Lab01-01.exe:**
- Sleep
- CreateProcessA
- CreateMutexA
- CloseHandle
The dll also takes imports from WS2_32.dll, which provides network functionality

**Lab01-01.dll:**
- CloseHandle
- UnmapViewOfFile
- IsBadReadPtr
- MapViewOfFile
- CreateFileMappingA
- CreateFileA
- FindClose
- FindNextFileA
- FindFirstFileA
- CopyFileA
This program seems to search for and create files.

**According to the textbook, CreateProcess and Sleep are common imports for backdoors.**

5. Are there any other files or host-based indicators that you could look for on infected systems?
*Read the strings in the files using pestudio or just the strings command*

**Lab01-01.exe:** There are strings containing ".text" and ".exe" in this PE, so it probably searches the system for files with that extension. Also, there's a string with "kerne123.dll". So maybe this malware is trying to replace the legit kernel32.dll file with this? That might be an indicator.

**Lab01-01.dll:** There's an IP address (127.26.152.13) in the file strings. Another interesting string is "exec". Also the word "hello" for some reason
*According to the textbook, CreateProcess can be used with exec in order to send commands through the backdoor to run a program. Sleep can be used to tell the backdoor to sleep for stealth.*

6. What network-based indicators could be used to find this malware on infected machines?
The IP address mentioned earlier in Lab01-01.dll

7. What would you guess is the purpose of these files?
*The .exe file seems to search for and replace kernel32.dll for a malicious version. The .dll seems to be a backdoor.*

# Lab 1-2
Analyze the file Lab01-02.exe.
## Questions
1. Upload the Lab01-02.exe file to http://www.VirusTotal.com/. Does it match any existing antivirus definitions?
Yes. (sha256: c876a332d7dd8da331cb8eee7ab7bf32752834d4b2b54eaa362674a2a48f64a6)

2. Are there any indications that this file is packed or obfuscated? If so, what are these indicators? If the file is packed, unpack it if possible.
*Open the file with PEiD. PEiD says that the file is packed with UPX.*

To unpack the file, download UPX. And decompressed with packed PE with the following command:

`upx -o [newFilename] -d "[pathToFile]"`

3. Do any imports hint at this program’s functionality? If so, which imports are they and what do they tell you?
Open the file in Dependencies.
- OpenMutexA: common for malware to have this
- CreateWaitableTimerA/SetWaitableTimer: seems sussy
- CreateService/OpenSCManagerA: run malware as service
- InternetOpenUrlA: open malicious link?

4. What host- or network-based indicators could be used to identify this malware on infected machines?
Strings the file to find IP addresses. Found a url:

`http://www.malwareanalysisbook.com`

Also among the strings are "Malservice"; more evidence that this malware is meant to be run as a service.

# Lab 1-3
Analyze the file Lab01-03.exe.
## Questions
1. Upload the Lab01-03.exe file to http://www.VirusTotal.com/. Does it match any existing antivirus definitions?
Yes (sha265: 7983a582939924c70e3da2da80fd3352ebc90de7b8c4c427d484ff4f050f0aec)

2. Are there any indications that this file is packed or obfuscated? If so, what are these indicators? If the file is packed, unpack it if possible.
Open the file with PEiD. This PE is packed with FSG 1.0. You need to use a debugger to unpack this file. I found a [blogpost](https://www.aldeid.com/wiki/Category:Digital-Forensics/Computer-Forensics/Anti-Reverse-Engineering/Packers/FSG) that teaches you how to do it.

3. Do any imports hint at this program’s functionality? If so, which imports are they and what do they tell you?
Trying to view imports in pestudio or Dependencies only shows:
- LoadLibraryA
- GetProcAddress
And the only library is KERNEL32.dll. So you can't view anything more unless you unpack this file.

4. What host- or network-based indicators could be used to identify this malware on infected machines?
The strings here don't have anything useful here. The textbook says that we'll revisit this later, so I guess I'll update this whenever I get to that section.

# Lab 1-4
Analyze the file Lab01-04.exe.
## Questions
1. Upload the Lab01-04.exe file to http://www.VirusTotal.com/. Does it match any existing antivirus definitions?
Yes. (sha256: 0fa1498340fca6c562cfa389ad3e93395f44c72fd128d7ba08579a69aaf3b126)

2. Are there any indications that this file is packed or obfuscated? If so, what are these indicators? If the file is packed, unpack it if possible.
Open the file with PEiD. PEiD doesn't detect a packer.

3. When was this program compiled?
Fri Aug 30 22:26:59 2019 (UTC)

4. Do any imports hint at this program’s functionality? If so, which imports are they and what do they tell you?
- WinExec
- WriteFile
- CreateRemoteThread
- MoveFileA
- GetCurrentProcess
- OpenProcess
- OpenProcessToken
- LookupPrivilegeValueA
- AdjustTokenPrivileges
- CreateFileA
**ADVAPI32.dll:** The program is messing with permissions

WriteFile and WinExec shows that it creates a file and executes it.

5. What host- or network-based indicators could be used to identify this malware on infected machines?
Look through the strings
- http[:]//www.practicalmalwareanalysis.com/updater.exe
- URLDownloadToFile
- winlogon.exe
- urlmon.dll
- sfc_os.dll
- psapi.dll
- \system32\wupdmgr.exe
It seems like the malware is downloading these files from the first link.

**Textbook notes:** wipdmgr.exe indicates that the program can create or modify a file at \system32.

1. This file has one resource in the resource section. Use Resource Hacker to examine that resource, and then use it to extract the resource. What can you learn from the resource?
Open the file in Resource Hacker. It turns out there's a whole other binary in the resource section. You can extract this resource by right clicking on it to save the resource.

Once I saved this, I opened it in pestudio. Here you can see the URLDownloadToFile being imported. In the initial binary, it was there as a string instead of an import. So this is the downloader part of the malware.

# Lab 3-1




