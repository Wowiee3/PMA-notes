**Dependency Walker Tool**
Can be used to explore dynamically linked functions.
The tool can be used to list out the DLLS being imported into the program. Clicking on a DLL will show its imported functions (the panel on the top). The panel at the bottom will show the functions that can be imported (but aren't used in this program). Also in these two windows, there are columns titled [[Ordinals]]. Some times malware imports functions by ordinal.
The remaining two panel display additional information about the DLLs and any reported errors. 

## Note
Dependency Walker stopped its development a long time ago, and has a couple of issues with running on modern systems. If it hangs, either check out the fix in [this thread](https://stackoverflow.com/questions/40549702/dependency-walker-hangs) or use [Dependencies](https://github.com/lucasg/Dependencies), a modern version of Dependency Walker.