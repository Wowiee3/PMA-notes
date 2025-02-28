# Part 2 | Malware Analysis in Virtual Machines
- Malware has to be run in a safe environment so it doesn't spread
- Physical machines on air gapped networks can be used
	- Air gapped meaning not connected to the internet or any other network. The downside being that some malware components rely on an internet connection to download parts, so analysis may be limited.
	- Most malware analysis is just done in a VM cus it's easier
## Creating a VM for Malware Analysis
Honestly this should have already been done before doing the first labs LOL

Just use VMware and install FlareVM on it. Make sure VMware tools are installed. If you need to make the VM air gapped, just remove the network adapter in the VM settings.
### Host-Only Networking
Creates a private LAN between the VM and the host. The VM can only connect to your host, but not the internet.

Make sure the host is fully patched, and maybe use a firewall to restrict connections from the VM to your host.

## Taking Snapshots
Snapshots can be used to rollback versions of your VM, in case you actually want to use the VM as a sandbox and run the malware. 

To take a snapshot in VMware, just right click the machine > Snapshot > Take Snapshot. The Snapshot Manager can be used to view and rollback snapshots.