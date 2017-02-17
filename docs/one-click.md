# Autolab + Tango OneClick Installation

OneClick is the fastest way to install Autolab and Tango on an Ubuntu VM. The installation uses packages Autolab, MySQL, and Tango into seperate Docker containers with specific exposed ports for communication.

There are two types of installations. A local development setup and a real-world ready setup that requires SSL certificates, email service configuration, and domain name registration. Use the local setup for experimentation before deploying in a real-world scenario on such apps like Heroku, EC2, or DigitalOcean, among others.

## Local OneClick Setup

### Overview
The only 3 steps you will go through:

* Prepare a ubuntu VM
* Download the installation package
* One command Install


### 1. Prepare an Ubuntu VM
These installation instructions are for Ubuntu. If you're on other operating system, we recommend you set up an Ubuntu virtual machine first with [Virtual Box](http://www.ubuntu.com/download/alternative-downloads).

About the System Configuration:

* [Ubuntu 14.04( or higher) 64bit](https://www.virtualbox.org/wiki/Downloads)
* 2GB memory + 20GB disk

To set up, [Install Ubuntu on Virtualbox](http://www.wikihow.com/Install-Ubuntu-on-VirtualBox) may help you.


**Optional:**

For better experience, we also recommend you to "insert guest additional CD image" for your virtual machine to enable full screen. 
(If you installed Ubuntu 16+, you can skip this)

```
Devices > Insert guest additional CD image
```

Also enable clipboard share for easier copy and paste between host and VM.

```
Settings > Advanced > Shared Clipboard > Bidrectional
```

You need to restart your virtual machine to validate these optional changes.


### 2. Download 

Root is required to install Autolab:

```bash
sudo -i
```

Clone repo:

```bash
git clone https://github.com/autolab/autolab-oneclick.git; cd autolab-oneclick
```

### 3. Installation
Run the following in the autolab-oneclick folder
```bash
./install -l
```
This will take a few minutes. Once you see ``Autolab Installation Finished``, ensure all docker containers are running:

```bash
docker ps
# CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                    PORTS                     NAMES
# c8679844bbfa        local_web           "/sbin/my_init"          3 months ago        Exited (0) 3 months ago                             local_web_1         721 kB (virtual 821 MB)
# 45a9e30241ea        mysql               "docker-entrypoint..."   3 months ago        Exited (0) 3 months ago   0.0.0.0:32768->3306/tcp   local_db_1          0 B (virtual 383 MB)
# 1ef089e2dca4        local_tango         "sh start.sh"            3 months ago        Exited (0) 3 months ago   0.0.0.0:8600->8600/tcp    local_tango_1       91.1 kB (virtual 743 MB)
```

Now Autolab is successfully installed and running on your virtual machine.
Open your browser and visit ``localhost:3000``, you will see the landing page of Autolab.


We have populated some dummy data for you to play around Autolab as an instructor.

```bash
email: admin@foo.bar
password: adminfoobar
```

### Teardown

To clean up the initial populated data

```
cd local
docker-compose run --rm -e RAILS_ENV=production web rake autolab:depopulate
```