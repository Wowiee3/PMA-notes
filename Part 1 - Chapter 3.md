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

```
C:\> rundll32.exe [dllName], [exportArguments]
```

`[exportArguments]` has to be a function name or ordinal from the exported function table in the DLL. In order to view the exported function table, you can use PEview/PE Explorer/pestudio

If it's a function name, you can just supply the function name as the argument. If it's an ordinal, it has to be written as `#[ordinalNumber]`.

Malicious DLLs run most of their code in DLLMain (the DLL entry point). This is executed whenever the DLL is loaded. So just using rundll32.exe will cause it to run. 
### Running DLL malware as a Service
Look for exports that indicate that the DLL is meant to be used as a service. Look for exports such as "InstallService", which as an example, you can run like this:

```
C:\> rundll32.exe [dllName], InstallService [serviceName]

C:\> net start [serviceName]
```

^^ofc, the above example only works provided there is an exported function called InstallService.
#### Service Malware missing an Install Function
You may need to install the service manually. In order to do this, use the Windows `sc` command, or modify your services registry at `\HKLM\SYSTEM\CurrentControlSet\Services` to include the new service, then run `net start` for that service.

### Analyzing network connections
Use ApateDNS to see DNS requests made by the malware. 
	Use the # of NXDOMAINs option if you want to see if the malware loops through a list of domains if the others aren't found.

Use netcat's listen option to check outbound connections
```
C:\> nc -l -p 80
```
^^ port 80. Most malware goes out port 80 or 443 since they're typically not blocked

You can combine ApateDNS and netcat. First use ApateDNS to redirect DNS requests to your local host. Then use netcat to listen for connections before running the malware.
^^ This is good for catching reverse shells

If your malware is looking for a specific service (SMTP, FTP), you can use INetSim on a Linux machine to simulate the service. Use ApateDNS to redirect the DNS request to the machine running INetSim and it'll record the requests it gets.

## Basic dynamic analysis environment setup
**Before running:**
- [ ] Run procmon and set a filter for the malware's name
- [ ] Start process explorer
- [ ] Take the first snapshot of the registry with registry explorer
- [ ] Set up ApateDNS and INetSim/netcat
- [ ] Set up wireshark to log network traffic

## Tools
- [[(TOOL) Process Monitor]]
- [[(TOOL) Regshot]]
## Labs
- [[(LAB) Part 1]]