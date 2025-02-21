- The panel on the left shows the main parts of the PE's header
- Selecting a header will open it up and show its values

See [[PE File Headers and Sections]] for more information on specific headers. 

# Tips
- All Delphi programs have a compile time of `June 19, 1992.`
- Most malware writers will fake the compile time to confuse you.
- IMAGE_OPTIONAL_HEADER will usually have some useful information.
	- The subsystem desc will indicate if the program is CLI or GUI
		- CLI: `IMAGE_SUBSYSTEM_WINDOWS_CUI`
		- GUI: `IMAGE_SUBSYSTEM_GUI`
- `IMAGE_SECTION_HEADER` contains information about each section of the program. The compiler generates the names of the sections, so they're usually consistent across executables. 
	- Section sizes can help detect packed programs. (e.g. if virtual size > raw data size, the section (often .text, but this is normal for .data in Windows programs) takes up more space in memory than it does on disk)