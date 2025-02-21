
# PE Headers

## Information in Header (Summary)
- Imports
- Exports
- Time program was compiled
- Sections
- Subsystem (CLI or GUI program)
- Resources

## IMAGE_NT__HEADERS
Shows NT headers. Includes IMAGE_FILE_HEADER, File signature, and IMAGE_OPTIONAL_HEADER
### IMAGE_FILE_HEADER
Contains information about the file (architecture, sections, symbols, date of creation)
- Machine: program architecture
- Time Date Stamp: when the program was compiled
### IMAGE_OPTIONAL_HEADER
PE loaders look here for specific information to load and run the executable. 
### IMAGE_SECTION_HEADER
Describes each section of a PE file.


# PE Sections
## .text
Contains the instructions that the CPU executes. 

## .rdata
Typically contains import and export information. 
Can also store other read-only data.
Sometimes, a program will contains `.idata` and `edata` which stores import and export information.

## .data
Contains the program's global data.

## .rsrc
Includes resources used by the program (icons, images, menus, strings). 

## .reloc
Contains information for relocation of library files. 

# Tips
See [[(TOOL) PEview]] for tips on analyzing malware headers and sections.