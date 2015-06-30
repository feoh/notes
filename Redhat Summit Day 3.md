## Talk: Redhat Satellite Advanced Users Tips & Tricks

* History
	* Original version of Satellite was monolithic - Frankenstein's monster
	* Satellite 5 - full featured but not "cloud scale" - upstream project was Spacewalk
	* Satellite 6 - full rewrite based on a number of projects - Katello, Foreman, MongoDB - 100% Open Source
* Installation
	* Kickstart system with 'base' package group
	* Be sure you can resolve all the needed hostnames
	* Be sure that time synchronization is set up correctly
	* SELinux should be enforcing
	* Be sure that the firewall is properly configured
	* Install Katello
		* Run Katello installer by itself
		* The Katello installer can be run as many times as needed
	* Organizations
		* Choose a name and create your organizations
		* Delete the default one
	* Register with Redhat
		* Assign subscriptions to this Satellite server
	* Manifest
		* Create manifest and assign products
	* Repositories
		* Link repositories, local or upstream e.g. EPEL, Puppet, etc.
		* Select repositories
	* Synchronizations
		* Choose repos to sync
		* For RHEL 7 it's like 50G
		* Sync plans - no more shell scripts or Cron jobs
		* Sync status - sync complete
	* Locations
		* Could be per data center
		* Ensure each location is associated with your Redhat organization
	* Lifecycle
		* Systems can be subscribed to different groups - Library, Dev, QA, Staging, Prod
		* Packages progress from one group to the next
		* Promotion is tracked and audited "When did we promote package X into production?"
	* Filtering
		* Packages can be excluded from systems based on a given set of criteria
			* e.g. "No devtools in Prod <Hi Jonah :)>"
		* Packages can be locked to a specific version or "Take the latest"
	* Host Collections
		* Arbitrary grouping of hosts
		* Can be based on location, but also any other factor you want
			* e.g. Wakefield Storage Servers
	* Installation Media
		* Associate location, organization etc to make Kickstart available
	*Templates
		* Provisioning Templates
			* Kickstart-default-iPXE
			* Be sure to associate with any families you want to kickstart
		* PXE Templates
		* Partitioning Templates
	* Infrastructure
		* Can use any dirt / cloud provider - RH, VMWare, EC2, QEMU, many more


	## Talk: Open Source and Network Function Virtualization
	* What is it?
		* Telcos are replacing specialized hardware with commodity hardware running VMs
		* Industry becoming increasingly competitive
	* Kinds of hardware being replaced
		* CDN, NAT, Firewall, load balancers, deep packet inspection, etc.
	* Telcos want to run their services in the private cloud
	* Virtualization
		* Compute - libvirt, QEMU, KVM
		* Storage - Ceph
		* Network - OpenDaylight, DPDK (Data Plane Development Kit), OpenVSwitch
	* Gaps in Open Source for NFV
		* VMs need to be able to be VLAN aware
		* Service Function Chaining
		* Deterministic Latency
		* Accelerated Data Plane
		* Federation of VMs
		* Better, quicker fault detection and reaction
	* [OPNFV](http://opnfv.org)
		* Broad industry support
		* Upstream First - improve the existing projects rather than creating separate "Telco grade" projects
			* "Fork Free Zone"
		* Working to fill in the gaps
		* Processing *200 MPPS* (Million Packets Per Second) with SRIOV
