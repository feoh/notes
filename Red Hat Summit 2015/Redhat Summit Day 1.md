# RedHat summit day 1: General Session

## First keynote
* Paul Cormier
  * [VMWare rolling out Linux container operating system - huge turnaround](http://blogs.vmware.com/cloudnative/introducing-photon/)
  * Linux runs the cloud - not arguable, it's a fact.
  * Redhat brought virtualization via KVM to the enterprise
  * Open source ecosystems accelerated the development of apps that solved hard problems, couldn't have been solved by proprietary dev
  * RHEL Security certification has been extended to containers
* Stephen Lucas -SAP President- Mostly boring, they're into open source as well
* Doug Fisher - Intel - yay Open stack. Intel is part of the Open Container project.

## Talk: Securing The Enterprise with Key cloak 
* It's a JBoss single sign on provider
*  Web, mobile, and IoT 
* Token based
*  Open ID & SAML
* RESTFUL via JSON
* Web app integration via a single tag 
* Client adaptors for all kinds of languages and frameworks
* Rich admin consoles for auditing, user management, etc. 
 
## Talk: Performance Optimization and Monitoring

* NUMA - Non Uniform Memory Access - for recent RH kernels
* As opposed to old UMA system
* NUMA helps with virtualization 
* NUMA handles paging, swapping behavior differently
	* Tools
		* SpecCPU
		* lscpu
		* top - hit 1 per CPU, 2 per NUMA node, 3 picks which NUMA node
		* [tuned - kernel tunable profiles pre-made ready to go](http://servicesblog.redhat.com/2012/04/16/tuning-your-system-with-tuned/)
		* numactl
* Linux uses memory aggressively for the page cache
* Kernel memory tunable for NUMA
	* zone_reclaim_mode
* NUMA Alignment tuning
	* numad - optional in RHEL 6 - tries to move process to node with resources
* LInux Scheduler
	* Kernel tunable can be changed in RHEL 6 without rebooting
		* /proc/sys exposes them

## Talk: Redhat's Container Strategy
 
* Container strategies can lead to more choice on the part of apps teams as to when & how to change their contents
* Increased complexity managing the contents
* 60% of containers in Docker repository contain medium priority security issues
* 35% of them contain High prio. security issues
* RedHat container certification helps alleviate this problem by certifying containers in their repository as secure
* RedHat containers are integrated with SELinux - each container runs inside its own policy context
* RedHat helped found OpenCintainer foundation based on code donated by Docker for an open container format (subset of Linux Foundation)
* Redhat Atomic Enterprise adds orchestration through Kubernetes
* Redhat Atomic Enterprise
	* Introspection, Lifecycle Management, Resiliency
	* Each individual container is immutable - make a change, old container gets thrown away  <-- Love this.
* Kubernetes enables "container fabric" - containers can run on top of OpenStack or 'bare metal' or even cloud services - e.g. Google, Amazon, whatever.
* Containers become more portable and easier to move from infrastructure type to infrastructure type
* Openshift - developing and deploying containers - will monitor source control for changes, rebuild, redeploy container, dev web UI

## Talk: RHEL Performance Tuning for Databases
* Tuned comes with RHEL 7 out of box
* I/O profiling
	* Use iostat command - watch queue size and request size - you want to limit queue size
	* iostat -dmxz <interval>
	* IO Elevators
		* Deadline - MP apps and systems running enterprise storage
		* CFQ - Default setting
		* Noop - Best for SSD
		* Use tuned to configure
	* Block device read-ahead - larger yields better perf for DBs
	* Database memory layout
		* Use vmstat
	* Better perf out of traditional non solid state IO devices
		* DM-Cache - tech preview in RHEL 6.5, stock in RHEL 7.2
	* NUMA and databases
		* Ideally each database has its own NUMA core
			* numactl --hardware will show you layout
		* AutoNuma - can actually hurt performance with large instances as it will try to move the db around between Numa nodes.
	* Always turn on Huge Pages!
	* Starting in RHEL 6, no need to reduce 'snappiness' parameter
	* Enabling Hyperthreads alone will not necessarily boost performance
		* However, Enabling Hyperthreads *and* aligning NUMA yields a nice performance boost.
* Network Performance
	* Private Network for database traffic
	* if on same network, use arp_filter to prevent ARP flux
	* echo 1>/proc/sys/net/ipv4/conf/all/arp_filter
	* Tool for monitoring network performance: war -n <interval> check for small request sizes
	* Jumbo Frames can boost network performance for large data payloads
		* Set 1500 MTU
* If running Oracle, use larger Redo logs
* Never use KSM - Kernel Same page Merging
* For VMs
	* VirtIO
	* PCI Passthrough
	* SR-IOV
* [Perf - performance monitoring tool](https://en.wikipedia.org/wiki/Perf_(Linux))
* Virtualization generally imposes a 5-15 percent performance penalty
