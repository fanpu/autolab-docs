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

* [REST API docs](/tango-rest)
* [VMMS API docs](/tango-vmms)
* [Tango Architecture Overview](http://autolab.github.io/2015/04/making-backend-scalable/)

### Installation

1. Obtain the source code.
    
        :::bash
        git clone https://github.com/autolab/Tango.git; cd Tango
    
2. Install Redis following [this guide](http://redis.io/topics/quickstart). By default, Tango uses Redis as a stateless job queue. Learn more [here](http://autolab.github.io/2015/04/making-backend-scalable/).

3. Create a `config.py` file from the given template. 
        
        :::bash
        cp config.template.py config.py

4. Create the course labs directory where job's output files will go, organized by key and lab name.

        :::bash
        mkdir courselabs
   By default the `COURSELABS` option in `config.py` points to the `courselabs` directory in the Tango directory.
   Change this to specify another path if you wish.

5. Set up a VMMS for Tango to use. The easiest and recommended is Docker.

   * [Docker](/tango-vmms/#docker-setup)















