Shows running processes in a tree structure so you can see parent/child processes.

- **Pink:** service
- **Blue:** process
- **Green:** new processes
- **Red:** terminated processes

Click the button next to the refresh button to show the bottom panel. Here when you select a process, you can see its Handles, DLLs, and Threads

Double clicking a process opens the properties window. Here you can see stuff like active connections, path on disk to the process, and stats about the process. You can even see strings here.

## Verify processes
Malware likes to disguise itself as legitimate Windows programs. Proc Explorer allows you to verify programs that may or may not be legit. To do this, open the properties window for a process, and click the verify button on the Image tab.

Note that this button only verifies the image on disk rather than memory. Meaning that if the attacker uses *process replacement*, where they run a process on the system and overwrite its memory with a malicious program. 

### Recognizing process replacement
In the properties window of a process, the strings tab can be used to check for process replacement. In the bottom left corner, there's an option to toggle the strings view to look at the strings of the process on disk and the process in memory. If they're different, it's probably process replacement.

## Finding DLLs
On the main window, you can Find > Find Handle or DLL to search for handles or DLLs. This is useful to search for programs using malicious DLLs.

Note that the verify button mentioned earlier only verifies the program, but not its DLLs. 

To check if a DLL is loaded into a process after its load time, you can compare the DLL listing in proc explorer to the import table from Dependencies/DependencyWalker.

## Analyzing Malicious Documents
Malicious documents can launch processes to do sussy stuff. To check if a document is malicious, you can use proc explorer to check for any processes opened by the document, and locate the malware on disk from the Image tab.