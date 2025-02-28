# Lab 1-1
This lab uses the files Lab01-01.exe and Lab01-01.dll. Use the tools and techniques described in the chapter to gain information about the files and answer the questions below.
## Questions
1. Upload the files to http://www.VirusTotal.com/ and view the reports. Does either file match any existing antivirus signatures?
		*Lab01-01.exe: Yes (sha256: 58898bd42c5bd3bf9b1389f0eee5b39cd59180e8370eb9ea838a0b327bd6fe47)*
		*Lab01-01.dll: Yes (sha256: f50e42c8dfaab649bde0398867e930b86c2a599e8db83b8260393082268f2dba)*
2. When were these files compiled?
		*Open the files in PEview (In my case, I used pestudio and it still worked fine)*
		*Lab01-01.exe: Sun Dec 19 16:16:19 2010 (UTC)*
		*Lab01-01.dll: Sun Dec 19 16:16:38 2010 (UTC)*
3. Are there any indications that either of these files is packed or obfuscated? If so, what are these indicators?
		*Open the files with PEiD*
		*Lab01-01.exe: PEiD didn't detect a packer*
		*Lab01-01.dll: PEiD didn't detect a packer*
4. Do any imports hint at what this malware does? If so, which imports are they?
		*Use Dependency Walker to check for imports. Dependency Walker was giving me some errors so I used the newer Dependencies program instead*
		*In the program, double click the file you loaded on the top left corner to see its imports*
		*Lab01-01.exe:*
		- Sleep
		- CreateProcessA
		- CreateMutexA
		- CloseHandle
		The dll also takes imports from WS2_32.dll, which provides network functionality
		*Lab01-01.dll:*
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
		*Lab01-01.exe: There are strings containing ".text" and ".exe" in this PE, so it probably searches the system for files with that extension. Also, there's a string with "kerne123.dll". So maybe this malware is trying to replace the legit kernel32.dll file with this? That might be an indicator.*
		*Lab01-01.dll: There's an IP address (127.26.152.13) in the file strings. Another interesting string is "exec". Also the word "hello" for some reason*
		**According to the textbook, CreateProcess can be used with exec in order to send commands through the backdoor to run a program. Sleep can be used to tell the backdoor to sleep for stealth.**
6. What network-based indicators could be used to find this malware on infected machines?
		*The IP address mentioned earlier in Lab01-01.dll*
7. What would you guess is the purpose of these files?
		*The .exe file seems to search for and replace kernel32.dll for a malicious version. The .dll seems to be a backdoor.*

# Lab 1-2
Analyze the file Lab01-02.exe.
## Questions
1. Upload the Lab01-02.exe file to http://www.VirusTotal.com/. Does it match any existing antivirus definitions?
2. Are there any indications that this file is packed or obfuscated? If so, what are these indicators? If the file is packed, unpack it if possible.
3. Do any imports hint at this program’s functionality? If so, which imports are they and what do they tell you?
4. What host- or network-based indicators could be used to identify this malware on infected machines?