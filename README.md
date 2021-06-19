## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Project_Diagram](Images/ProjectDiagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the .yml file may be used to install only certain pieces of it, such as Filebeat.

  - [DVWA Deployment playbook](Resources/dvwa-playbook.yml)
  - [ELK Deployment playbook](Resources/elk-playbook.yml)
  - [Filebeat Deployment playbook](Resources/filebeat-playbook.yml)
  - [Metricbeat Deployment playbook](Resources/metricbeat-playbook.yml)

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly avaliable, in addition to restricting access to the network.

- A Load balancer is meant to serve the incoming traffic. It act as a specific point of access for a service that is served by multiple machines. This allows availability model to function properly.
- A jumpbox server serves as a gateway to connect to the data servers. This way, data servers is not directly exposed to the public. Only the desitnated administrator can access through ssh (and this can also be more restricted to designated administrator by creating the firewall rule (Network Security Group))


Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log files and system resources.

- Filebeat watches for change in system logs and forwards any changes to the Elasticsearch Host
- Metricbeat is used only for gathering metrics and system resources usage to display in Elasticsearch.


The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name         | Function   | IP Address|Operating System |
|:------------:|:----------:|:---------:|:---------------:|
| Jump Box     | Gateway    | 10.0.0.4  | Linux / Ubuntu  |
| Web-1 VM     | Web Server | 10.0.0.5  | Linux / Ubuntu  |
|    Web-2 VM  | Web Server | 10.0.0.6  | Linux / Ubuntu  |
|   DVWA-VM3   | Web Server | 10.0.0.7  | Linux / Ubuntu  |
| ElkServer VM | ELK Server | 10.1.0.4  | Linux / Ubuntu  |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the Public IP address of the Administrator (**which is the author of this Project: Sooji Lee**)


#### Machines within the network can only be accessed by Jumpbox.
- Configuration of the ElkServer VM could be done through Jumpbox via ssh
- Kibana could be accessed through Sooji Lee's public IP (Whitelisted)
- *Jumpbox*
  - Public IP: 13.91.254.174
  - PrivateIP: 10.0.0.4
- *ElkServer VM*
  - Public IP: 51.143.5.106
  - PrivateIP: 10.1.0.4


A summary of the access policies in place can be found in the table below. </br>
*Please note that the Administrator's Public IP was allowed through Network Securigy Group (NSG) configuration setting*

| Name     | Publicly Accessible | Allowed IP Addresses |
|:--------:|:-------------------:|:--------------------:|
| Jump Box | Yes (SSH)           | Administrator's Public IP   |
| Web-1 VM |     No              |          None        |
| Web-2 VM |        No           |          None        |
| DVWA-VM3 |          No         |          None        |
| LoadBalancer |    Yes (HTTP)   |  Administrator's Public IP  |
| ElkServer VM |  Yes (KIBANA)   |  Administrator's Public IP  |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- This allowed for automation of deploying of a container.
- By deploying container you need with automation, you can reduce any human error
- Also, you can save time by deploying multiple containers at once.

The playbook implements the following tasks:
- Install Docker: installs the docker engine to be run on the Data Server
- Install Docker container: Installs container using Docker. Also uses the image of the file to install specific container admin is looking for.
- Increase Memory/Use More Memory: When installing ELK Docker with the image, it needs enough memory to function well. This helps fix the issue of not enough memory and allows the server to launch with no problem.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![dockerps](Images/dockerps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Webserver machines with following IPs:
  - 10.0.0.5
  - 10.0.0.6
  - 10.0.0.7

We have installed the following Beats on these machines:
- All the Webserver machines were installed with Filebeat & Metricbeat:
  - Web-1 VM
  - Web-2 VM
  - DVWA-VM3

These Beats allow us to collect the following information from each machine:
- Filebeat collects system logs such as login events to see who has actively logged into the system.
- Metricbeat collects metric information such as CPU usage and memory. This is useful to see if there are any behaviours with system resources.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the [elk-playbook.yml](Resources/elk-playbook.yml) file to `/etc/ansible/roles/elk-playbook.yml`.
- Update the hosts file to include the IP of the webservers & elk servers.
- Run the playbook, and navigate to `http://<your_elk_server_ip>:5601/app/kibana` to check that the installation worked as expected.

![kibana-homepage](Images/kibana-homepage.png)