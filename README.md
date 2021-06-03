## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](Images/ELK-DRAW.IO.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: Enter the playbook file._

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file systems of virtual machines on the network, as well as system logs.


The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name                 | Function         | IP Address | Operating System |
|----------------------|------------------|------------|------------------|
| Jump-Box-Provisioner | Gateway          | 10.0.0.5   | Linux            |
| Elk-Server           | Configuration VM | 10.3.0.4   | Linux            |
| Web-1                | DVWA Container   | 10.0.0.6   | Linux            |
| Web-2                | DVWA Container   | 10.0.0.7   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box-Provisioner server can accept connections from the Internet. Access to this machine is only allowed from the following IP address:
72.94.81.86 - local machine

Machines within the network can only be accessed by DVWA containers setup within the Jump-Box-Provisioner.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible | Allowed IP Address    |
|----------------------|---------------------|-----------------------|
| Jump-Box-Provisioner | Yes                 | 10.0.0.5, 72.94.81.86 |
| Elk-Server           | No                  | 10.0.0.5, 72.94.81.86 |
| Web-1                | No                  | 10.0.0.5              |
| Web-2                | No                  | 10.0.0.5              |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- ...
- ...

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/DockerPS-ELK.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
Web-1: 10.0.0.6
Web-2: 10.0.0.7

Have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_

These Beats allow us to collect the following information from each machine:

Filebeat detects changes to the filesystem and collects Apache logs.
Metricbeat detecs changes in system metrics, such as CPU usage. It can be used to detect sudo escalations, and CPU or RAM statistics.
Packetbeat collects packets that pass through the network interface card, similar to of Wireshark. It can be utilized to capture and generate a trace of all activity that takes place within the network.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbooks to the Ansible Control Node.
- Update the _____ file to include...
- Run the playbook, and navigate to ____ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._