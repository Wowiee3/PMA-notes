# Part 3 | Basic Dynamic Analysis
You should usually perform dynamic analysis after static analysis reaches a dead end. Performing static analysis will help with understanding the program better before you actually run the thing.

## Just use a Sandbox
Any.run is kinda the GOAT.
### Downsides of using a Sandbox
- No command line arguments
- Malware can have sandbox evading features
	- Waiting a long time before running
	- Stops running when sandbox is detected
	- Etc
## Running it by Yourself
Running exes are easy, but dlls can be tricky because Windows doesn't know how to run those automatically. 
### Running DLLs
The `rundll32.exe` program provides a container for running DLLs:

`rundll32.exe [dllName], [exportArguments]`

`[exportArguments]` has to be a function name or ordinal from the exported function table in the DLL. In order to view the exported function table, you can use PEview/PE Explorer/pestudio

If it's a function name, you can just supply the function name as the argument. If it's an ordinal, it has to be written as `#[ordinalNumber]`.

Malicious DLLs run most of their code in DLLMain (the DLL entry point). This is executed whenever the DLL is loaded. So just using rundll32.exe will cause it to run. 
### Running DLL malware as a Service
Look for exports that indicate that the DLL is meant to be used as a service. Look for exports such as "InstallService", which as an example, you can run like this:

`rundll32.exe [dllName], InstallService [serviceName]`

`net start [serviceName]`

^^ofc, the above example only works provided there is an exported function called InstallService.
#### Service Malware missing an Install Function
You may need to install the service manually. In order to do this, use the Windows `sc` command, or modify your services registry at `\HKLM\SYSTEM\CurrentControlSet\Services` to include the new service, then run `net start` for that service.

## Tools
- [[(TOOL) Process Monitor]]

## Labs