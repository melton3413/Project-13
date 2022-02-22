# Project-13
GE-GT-Cyber-Project-13

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![](https://github.com/melton3413/Project-13/blob/main/Diagrams/GE-GT-UNIT-13.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _yml_ and _config_ file may be used to install only certain pieces of it, such as Filebeat.

  - [ELK-install](https://github.com/melton3413/Project-13/blob/main/Ansible/install-elk.yml)
  - [Filebeat-playbook](https://github.com/melton3413/Project-13/blob/main/Ansible/filebeat-playbook.yml)
  - [Metricbeat-playbook](https://github.com/melton3413/Project-13/blob/main/Ansible/metricbeat-playbook.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly avaialble, in addition to restricting access to the network.
- Load balancers add resiliency by rerouting live traffic from one server to another if a server falls prey to DDoS attacks or otherwise becomes unavailable. In this way, load balancers help to eliminate single points of failure, reduce the attack surface, and make it harder to exhaust resources and saturate links.

- A jump box allows easier administration and protects virtual machines from exposure to the internet.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log files and system performance.
- Filebeat is a lightweight shipper for forwarding and centralizing log data. Installed as an agent on the servers, Filebeat monitors the log files for specific locations, collects log events, and forwards them to System logs for indexing.
- Metricbeat is a lightweight shipper to periodically collect metrics from the operating system and from services running on the server.  Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Docker metrics.

The configuration details of each machine may be found below.
<!-- Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table. -->

| Name       | Function   | IP Address | Operating System    |
|------------|------------|------------|---------------------|
| Jump Box   | Gateway    | 10.0.0.4   | Linux Ubuntu 20.04  |
| Web 1      | Server     | 10.0.0.5   | Linux Ubuntu 20.04  |
| Web 2      | Server     | 10.0.0.6   | Linux Ubuntu 20.04  |
| ELK Server | Log Server | 10.1.0.4   | Linux Ubuntu 20.04  |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Workstation IP Address - 99.x.x.x

Machines within the network can only be accessed by the Jump Box.
- The ELK Server is only accessible by the Workstation IP Address on port 5601.

A summary of the access policies in place can be found in the table below.

| Name          | Publicly Accessible | Allowed IP Addresses |
|---------------|---------------------|----------------------|
| Jump Box      | Yes                 | 99.x.x.x             |
| Load Balancer | Yes                 | Open                 |
| Web 1         | No                  | 10.0.0.4             |
| Web 2         | No                  | 10.0.0.4             |
| ELK Server    | Yes                 | 99.x.x.x             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Ansible automation helps considerably with the representation of Infrastructure as Code (IAC). IAC involves provisioning and management of computing infrastructure and related configuration through machine-processable definition files.

The [ELK-install](https://github.com/melton3413/Project-13/blob/main/Ansible/install-elk.yml) playbook implements the following tasks:

- Install docker.io
- Install python-pip
- Install docker python module

```bash
    # Use apt module
    - name: Install docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present

      # Use apt module
    - name: Install pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present
      
      # Use pip module
    - name: Install Docker python module
      pip:
        name: docker
        state: present
``` 

- Use sysctl module to increase the virtual memory for the ELK VM
```bash
      # Use sysctl module
    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes
```

- Use docker container module to download ELK VM with custom ports
```bash
      # Use docker_container module
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
```

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![](https://github.com/melton3413/Project-13/blob/main/Images/elk_vm_docker_ps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web 1 - 10.0.0.5
- Web 1 - 10.0.0.6

We have installed the following Beats on these machines:
- filebeat-7.4.0-amd64.deb
- metricbeat-7.4.0-amd64.deb

These Beats allow us to collect the following information from each machine:
- Filebeat is collecting web server syslog events and processes.
- Metricbeat collects metrics and statistics such as CPU usage, Memory usage, and Disk I/O.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the config file to Web VMs.
- Update the /etc/ansible/hosts file with IP Addresses for the ELK and Web servers.
- Run the playbook, and navigate to http://[ELK_VM_Public_IP]:5601/app/kibana to check that the installation worked as expected.

- _Which file is the playbook?_  filebeat-playbook.yml
- _Where do you copy it?_  to the ELK server.
- _Which file do you update to make Ansible run the playbook on a specific machine?_  filebeat-config.yml
- _How do I specify which machine to install the ELK server on versus which to install Filebeat on?_  Update the hosts file to add/remove IP addresses for different servers.
- _Which URL do you navigate to in order to check that the ELK server is running?_  http://[ELK_VM_Public_IP]:5601/app/kibana

[filebeat-playbook.yml](https://github.com/melton3413/Project-13/blob/main/Ansible/filebeat-playbook.yml)
```bash
- name: Installing and Launch Filebeat
  hosts: webservers
  become: yes
  tasks:
    # Use command module
  - name: Download filebeat .deb file
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb

    # Use command module
  - name: Install filebeat .deb
    command: dpkg -i filebeat-7.4.0-amd64.deb

    # Use copy module
  - name: Drop in filebeat.yml
    copy:
      src: /etc/ansible/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

    # Use command module
  - name: Enable and Configure System Module
    command: filebeat modules enable system

    # Use command module
  - name: Setup filebeat
    command: filebeat setup

    # Use command module
  - name: Start filebeat service
    command: service filebeat start

    # Use systemd module
  - name: Enable service filebeat on boot
    systemd:
      name: filebeat
      enabled: yes
```

[metricbeat-playbook.yml](https://github.com/melton3413/Project-13/blob/main/Ansible/metricbeat-playbook.yml)
```bash
- name: Install metric beat
  hosts: webservers
  become: true
  tasks:
    # Use command module
  - name: Download metricbeat
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.4.0-amd64.deb

    # Use command module
  - name: install metricbeat
    command: dpkg -i metricbeat-7.4.0-amd64.deb

    # Use copy module
  - name: drop in metricbeat config
    copy:
      src: /etc/ansible/metricbeat-config.yml
      dest: /etc/metricbeat/metricbeat.yml

    # Use command module
  - name: enable and configure docker module for metric beat
    command: metricbeat modules enable docker

    # Use command module
  - name: setup metric beat
    command: metricbeat setup

    # Use command module
  - name: start metric beat
    command: service metricbeat start

    # Use systemd module
  - name: Enable service metricbeat on boot
    systemd:
      name: metricbeat
      enabled: yes
```
