Procmon can be used to monitor:
- Registry
- File system
- Network (not that great for this)
- Process and thread activity

Procmon captures all system calls. Avoid running it for too long. 

## Running
Before using procmon, clear all captured events through Edit > Clear Display

Start and stop capture with File > Capture Events

## Display
Procmon gives a lot of information about events, including:
- Sequence number
- Timestamp
- Name of process that created the event
- Event operation
- Path used by event
- Result of event

## Filtering
You can set procmon to filter in one executable on the system, or certain syscalls. Setting a filter just prevents the other stuff from showing up in the display; other events will still be captured. This is useful in case you're filtering by process name, and the process executes another file. That event will still be captured.

To set a filter, go to Filter > Filter... (wow). Useful filters are for `Operation` and `Process Name`.

Filters can always be changed or removed to view other events.

## Tips
- Reading the `Operation` column will tell you the operation performed.
- There are 4 funny colored icons in the toolbar that filter in/out registry, file, network, and process activity events.