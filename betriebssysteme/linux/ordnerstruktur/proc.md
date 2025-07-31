# /proc virtual filesystem because kernel exposes runtime data.

## key files
```bash
/proc/cpuinfo           # CPU details - model, cores, flags
/proc/meminfo           # memory usage - total, free, buffers
/proc/uptime            # system uptime and idle time
/proc/version           # kernel version and build info
/proc/cmdline           # kernel boot parameters
/proc/mounts            # mounted filesystems
/proc/net/arp           # ARP table
/proc/net/tcp           # TCP connections
/proc/loadavg           # system load averages
```

## process directories
each PID has `/proc/[pid]/`:
```bash
/proc/1234/cmdline      # command line that started process
/proc/1234/environ      # environment variables
/proc/1234/fd/          # file descriptors (open files)
/proc/1234/maps         # memory mappings
/proc/1234/status       # process status info
/proc/1234/cwd          # current working directory (symlink)
/proc/1234/exe          # executable file (symlink)
```

## useful commands
```bash
cat /proc/cpuinfo | grep "model name"   # CPU model
cat /proc/meminfo | grep MemTotal       # total RAM
cat /proc/loadavg                       # current load
ls -la /proc/*/exe | grep deleted       # processes with deleted binaries
find /proc/*/fd -type l -ls 2>/dev/null | grep socket    # network sockets
```

## privilege escalation vectors
```bash
ls -la /proc/*/fd/ 2>/dev/null | grep -E "(log|shadow|passwd)"    # sensitive file handles
cat /proc/*/environ 2>/dev/null | grep -i pass                   # passwords in env vars
```

## notes
[!] virtual filesystem - exists only in RAM, not on disk
[!] provides kernel interface - no real files stored
[!] process info readable by process owner and root
[!] some info only visible to root (like other users' process details)