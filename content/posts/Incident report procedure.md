Linux Based Environment
Goal: identify faulty server to find the root cause, gather evidence, either resolve the issue or escalate it.
Assumptions
An incident has been declared and the server is suspected to be involved
The operations person has login credentials(SSH and password)
Existent of a ticketing or issue tracking system(Jira, ServiceNow)
phase1: preparation and initial assessment
Acknowledge the assigned ticket: take up the assigned ticket and read through the details
From this point, document every action carried out, command run and the timestamp for better documentation in the postmortem
Gather context: Understand the issues presented or associated with the server in the ticket(e.g ‘slow server’, ‘API returning 500’, ‘Users unable to login into the domain’ )
Understand the issue reported so as to know what to tackle
what errors are presented
what services are hosted in the server?
Are any other services in the server experiencing the same issue?
Check for monitoring dashboards before logging in(DataDog, Grafana, Nagios, ) look for obvious anomalies:
 High CPU/Memory Usage
Latency spikes
Error rate increase
Health check failures
Phase2: Basic Connectivity & system status
Connection test	
From your workstation or bastion host, attempt to host the server
Observervation: check for packet loss, latency or reachability
2. Login attempt: try logingin in using SSH or RDP
observation: successful login? slow login? connection refused? Timeout? Authentication error?
3. Initial system Overview when logged in:
check system uptime uptime
observation: How long has the system been up?  Has the system booted recently? what is the load average?
check for logged in users:  `who w` or
	Observation: Any unexpected user or session?
Recent Login History: last | head -n 20
Observation: check for any unusual logins around when the issue started
Phase 3: Resource Utilisation Analysis
CPU Usage: Check overall per-process CPU usage top htop if installed
observation: is the CPU usage close to 100%, which processes are consuming most CPU?
Run for a short period to see trends vmstat 1 5
Observation: Look at us , sys , id , wa high wa (IO wait time ) means that there are bottle necks

Memory Usage: check memory usage free -h
	observation: is available memory very low? is swap heavily used? 
use top or htop (sort by memory usage type M in top, or f6 in htop the select PERCENT_MEM)
observation: which processes are consuming alot of memory?
Check OOM(out of memory) killer events dmesg | grep -i oom-killer or journalctl | grep -i oom
observation; Has the kernel killed processes due to low memory?
Disk I/O: check for disk utilisation and wait times: iostat -dxz 1 5 (reports disk status every minute 5 times)
	observation: Look for high %util and high wait times. which devices are affected?
check per process I/O iotop 
observation: Check for processes with most disk writes

**Disk space**:
check file system usage: df -h
observation: any critical file mounts(/var , /temp , / ) full or almost full?(> 90-95) 
Network I/O clonnection
Check network interface statistics ip -s link show or `netstat -i`
observation: Look for high error count errors , dropped
Check network throughput per interface/ process: iftop or nethogs
observation: is a particular interface saturated? is there a process sending high abnormal traffick?
Listening ports and established connections: `ss -tulnp` `netstat -tulpn`
  observation: Are there any expected services listening? 
              Are there expected services listening?(`ESTABLISHED`, `TIME_WAIT`)
              Any suspicious listening port?
## Application and service level checks
- **service status**
  - check the status of the primary running apps/services on the server
  * systemd:  `system status [servicename]`
  * SysVinit: `service [servicename] status` or `/etc/init.d/[servicename] status`
  observation: is the servive active/running?
              Did the service exit with an error?
              check the recent logs provided by the `status` command
