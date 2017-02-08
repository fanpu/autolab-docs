# Tango

Tango is a standalone RESTful Web service that runs jobs in virtual machines or containers. It was developed as a distributed grading system for [Autolab](/) and has been extensively used for autograding programming assignments.

## Getting Started

A brief overview of the Tango respository:

* `tango.py` - Main tango server
* `jobQueue.py` - Manages the job queue
* `jobManager.py` - Assigns jobs to free VMs
* `worker.py` - Shepherds a job through its execution
* `preallocator.py` - Manages pools of VMs
* `vmms/` - VMMS library implementations
* `restful-tango/` - HTTP server layer on the main Tango

Tango runs jobs in VMs using a high level Virtual Memory Management System (VMMS) API. Tango currently has support for running jobs in Docker containers (**recommended**), [Tashi VMs](http://opencirrus.intel-research.net/tashi/), or Amazon EC2.

For more information about the different Tango components, go to the following pages:

