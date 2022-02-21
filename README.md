# Project-13
GE-GT-Cyber-Project-13

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![](https://github.com/melton3413/Project-13/blob/main/Diagrams/GE-GT-UNIT-12.png)

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
- Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
- Metricbeat collect metrics from the operating system and from services running on the server.

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

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _____ file to _____.
- Update the _____ file to include...
- Run the playbook, and navigate to ____ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
