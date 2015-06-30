# Redhat Summit Day 2 Notes

## Talk: From Atomic to Nulecule
* [Nulecule](https://github.com/projectatomic/nulecule) bundles applications and all their dependencies
* Nulecule can run on top of any orchestrator of your choice - like Kubernetes
* Run 'atomic install myapp'
	* Say you want to install Wordpress
		* Pulls Wordpress, MariaDB, webserver
		* Answerfiles - provide details to installers
* Cockpit - Web UI for app install - Still very alpha

## Talk: Demystifying Systemd
* Breaks system up into units and manages dependencies between them
* systemd relies on groups to kill both process and children for daemons
* Debuggability
* 'Easy to learn' & 99% backwards compatible
* systemctl replaces service, chkconfig
* Runlevels are deprecated - use Targets
* Socket Activation - listens on port, starts service on connect, on demand
	* Formerly part of inter
* Timers - lets services start up in a timed (interval?) fashion
* systems-delta - show all customizations installed on this system
* Scope is vastly larger than old school init
	* Uses Cgroups
	* CPU, memory, network configuration, limits, slicing - partition resources
	* RHEL 7 enforces Cgroups out of box, RHEL 6 not all processes might respect them
	* Services can be partitioned as well  - e.g. apache gets all the CPU, some bloat Java app only gets X memory etc.
	* cgtop tool
* Unit config files are declarative - no longer shell programs
	* easier to parse
	* can be read & modified by folks who can't code
	* Monitoring & tuning software can consume them because they're easily parsed
* The Journal - logging
	* Indexed, formatted
	* Filtering & query-ability
	* Structured format
	* Intelligently rotated
	* Security
		* A fake CGI script could claim to be MariaDB in old school syslog
		* In system there's the concept of trusted and untrusted fields
* nSpawn
	* Rocket based on it

## Talk: Container Security: Do Containers Contain, and do we care?
* SELinux super important
	* Docker without SELinux is like Tupperware without the burp :)
	* Everything that is not explicitly allowed is denied
	* Comes with thousands of allow rules
	* Every process gets a tag, processes running in container are tagged LXC_XXX
* Treat services inside containers just like outside
	* Crunchy on the inside crunchy on the outside
* root is root whether inside the container or out
* Mount container filesystems as read-only
* Linux Kernel Capabilities
	* Some Capabilities removed when running Docker
* Namespaces
	* PID namespace
	* Network Namespace
	* Device Cgroup (Should be namespace)
* Only RHEL images are examined by RHEL when used in containers - other distributions you're on your own.
* FUTURE
* seccomp
	* Allows kernel to filter 'dangerous' system calls and keep them from being used
	* Developed by Google for Chrome
* User Name Sace
	* Example: id 0 (root) inside the container maps to UID 5000 outside the container
	* Filesystem has no concept of it
		* Non mapped UIDs outside the container become UID -1
* People installing random Docker images is - duh. Dangerous :)
 
## Talk: SELinux for Mere Mortals
* SELInux history
	* Developed by NSA - added to the Linux Kernel in 2003
* Labeling and Type Enforcement are the two most important bits to remember for SELInux
* On RHEL, common policies are:
	* 'targeted' - certain apps are constrained, everything else runs unconfined
* Config in /etc/selinux/config
	* Use sestets to query state
	* get enforce for scripting
* Labeling
	* Files and directories - labels are stored in matter if file supports it
	* format: user role type level [optional]
	* ls -Z shows httpd httpd.exec for scripts
	* ps -Z will show labels for running processes
	* netstat -Z will show labeled ports
	* seaman port -l will show labels for ports
* Type enforcement
	* Process labeled with httpd type has no business interacting with shadow_t type (passwords)
* Tools
	* ls -Z, id -Z,  netstat -Z, ps -Z, cp -Z, mkdir -Z 
* Contexts
	* Contexts are inherited
		* Dir has label, files created in that dir inherit
		* login defines context, by default processes spawned from it inherit
* SELinux Errors
	* Often the labeling is wrong
	* Policy needs to be tweaked?
	* Could be a bug in the policy
	* Might represent a security breach
* Booleans
	* sebum -A to see them all
	* booleans.local will show you all the booleans that are set - don't edit it, it won't do anything.
	* Example: 
		* Should https be able to auth with Windows?
		* httpd homedirs?

* Troubleshooting
 	* Install setroubleshoot and setroubleshoot-server
	* Restart audit and it will start logging
	* Example: Fred wants to enable web pages in /home/fred
		* /var/log/access_log and error_log are useless
		* /var/log/messages will give you the exact instructions (user fred didn't have permissions. Use these commands to fix it etc.)
	* Say a user creates a file in their home directory, you, the admin, are asked to move it into the httpd content context.
		* That will be mislabeled user.home even though it's not in the system web content context, you'll get a 403.
		* chcon  --reference <correct dir>  will set the SELinux context of the file / directory in question based on the correct dir.
		* restore con restores the SELinux context to default
* Creating Policy Modules
	* audit2why
	* audit2allow
	* semodule -i
* Starting out
	* Set selinux to 'permissive' NOT 'enforce'
	* relabel filesystem
	* reboot
* Final thoughts
	* Don't turn it off
	* Can save you from a security breach
	* NSA level security for free
	* Go read Dan Walsh's blog
