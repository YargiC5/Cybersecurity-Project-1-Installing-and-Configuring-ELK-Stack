<h1>Automated ELK Stack Deployment</h1>

The files in this repository were used to configure the network depicted below.

[ELK Network Diagram](https://github.com/YargiC5/Cybersecurity-Project-1-Installing-and-Configuring-ELK-Stack/blob/main/Images/ELK-DRAW.IO.PNG)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


<h1>Description of the Topology</h1>

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file systems of virtual machines on the network, as well as system logs.


The configuration details of each machine may be found below.

| Name                 | Function         | IP Address | Operating System |
|----------------------|------------------|------------|------------------|
| Jump-Box-Provisioner | Gateway          | 10.0.0.5   | Linux            |
| Elk-Server           | Configuration VM | 10.3.0.4   | Linux            |
| Web-1                | DVWA Container   | 10.0.0.6   | Linux            |
| Web-2                | DVWA Container   | 10.0.0.7   | Linux            |

<h1>Access Policies</h1>

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box-Provisioner server can accept connections from the Internet.
Access to this machine is only allowed from the following IP address:
<ul>
<li>72.94.81.86 - local machine</li>
</ul>

Machines within the network can only be accessed by DVWA containers setup within the Jump-Box-Provisioner.

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible | Allowed IP Address    |
|----------------------|---------------------|-----------------------|
| Jump-Box-Provisioner | Yes                 | 10.0.0.5, 72.94.81.86 |
| Elk-Server           | No                  | 10.0.0.5, 72.94.81.86 |
| Web-1                | No                  | 10.0.0.5              |
| Web-2                | No                  | 10.0.0.5              |

<h1>Elk Configuration</h1>

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because automation provided efficiency, and less chance of a mess up.

- Playbook for ELK Installation and Configuration:
'''
---
- name: Configure Elk VM with Docker
  hosts: elkservers
  remote_user: elk
  become: true
  tasks:
    - name: Install docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present
    - name: Install pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

    - name: Install Docker python module
      pip:
        name: docker
        state: present

    - name: Increase virtual memory
      command: sysctl -w vm.max_map_count=262144

    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes

    - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        published_ports:
          - 5601:5601
          - 9200:9200
          - 5044:5044
'''

The playbook implements the following tasks:
<ul>
  <li>Installing Docker</li>
  <li>Configuring Containers</li>
  <li>Increasing Virtual Memory</li>
  <li>Downloading the Proper Images</li>
</ul>

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

[DockerPS](https://github.com/YargiC5/Cybersecurity-Project-1-Installing-and-Configuring-ELK-Stack/blob/main/Images/DockerPS-ELK.PNG)

<h1>Target Machines & Beats</h1>

This ELK server is configured to monitor the following machines:
<br></br>
Web-1: 10.0.0.6
<br></br>
Web-2: 10.0.0.7
<br></br>
Have installed FileBeat and MetricBeat on these machines.

- Playbook for Installing MetricBeat:
'''
---
- name: Install metric beat
  hosts: webservers
  become: true
  tasks:
  - name: Download metricbeat
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.4.0-amd64.deb

  - name: install metricbeat
    command: dpkg -i metricbeat-7.4.0-amd64.deb

  - name: drop in metricbeat config
    copy:
      src: /etc/ansible/files/metricbeat-config.yml
      dest: /etc/metricbeat/metricbeat.yml

  - name: enable and configure docker module for metric beat
    command: metricbeat modules enable docker

  - name: setup metric beat
    command: metricbeat setup

  - name: start metric beat
    command: service metricbeat start
'''

These Beats allow us to collect the following information from each machine:

- Filebeat detects changes to the filesystem and collects Apache logs.
- Metricbeat detecs changes in system metrics, such as CPU usage. It can be used to detect sudo escalations, and CPU or RAM statistics.
- Packetbeat collects packets that pass through the network interface card, similar to of Wireshark. It can be utilized to capture and generate a trace of all activity that takes place within the network.

<h1>Using the Playbook</h1>

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbooks to the Ansible Control Node.
- Run the playbook, and navigate to /etc/ansible/files to check that the installation worked as expected.
- Use the curl command to add the config file:
<code> curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/files/filebeat-config.yml </code>

- Update the filebeat-config.yml file to include the ELK server private IP in lines 1106 and 1806.
- Run the filebeat-playbook.yml playbook, and navigate to the kibana page at [ELK public IP]/app/kibana to check the installation.