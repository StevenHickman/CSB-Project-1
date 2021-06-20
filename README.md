# CSB-Project-1
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![](Diagrams/Network%20Image.png)

These files have been tested and used to generate a live ELK deployment on Azure.

  - DVWA Yaml.txt
  - Elk Yaml.txt
  - Filebeat Yaml.txt
  - Metricbeat Yaml.txt

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly resistant to downtime issue and have a much higher avaliblity, 
in addition to restricting public access points to the network.


Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the files and system information such as uptime
and cpu usage.


The configuration details of each machine may be found below.

| Name     | Function                      | IP Address                               | Operating System |
|----------|-------------------------------|------------------------------------------|------------------|
| Jump-Box | Gateway                       | public: 137.135.105.83 private: 10.0.0.4 | linux            |
| Web1     | webserver                     | private: 10.0.0.5                        | linux            |
| Web2     | webserver                     | private: 10.0.0.6                        | linux            |
| Web3     | webserver                     | private: 10.0.0.8                        | linux            |
| Elk      | monitoring and log collection | public: 52.247.202.16 private: 10.1.0.4  | linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can be controled through deivces outside the network. Access to this machine is only allowed from the following IP addresses:
107.215.14.159

Machines within the network can only be accessed by the Jump Box [10.0.0.4] through an ssh key stored in the ansiable container.


A summary of the access policies in place can be found in the table below.

| Name              | Publiacly Acessiable     | Allowed IP Addresses    | Open Ports |
|-------------------|--------------------------|-------------------------|------------|
| Jump-Box          | Yes                      | 107.215.14.159          | 22         |
| Web1              | No                       | 10.0.0.4                | 22 80 5601 |
| Web2              | No                       | 10.0.0.4                | 22 80 5601 |
| Web3              | No                       | 10.0.0.4                | 22 80 5601 |
| Elk               | No                       | 10.0.0.4                | 22 5601    |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous as it allows
for both a rapid and scalable deployment of the nessicary componets of the elk serve.

The playbook implements the following tasks:
- install docker.io
- install python-pip
- install the docker module with pip
- increase the virutal memory used
- download the elk image to the contaier and establish the published ports

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![](Linux/docker%20ps20-a%20results.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web1 [10.0.0.5]
- Web2 [10.0.0.6]
- Web3 [10.0.0.8]

We have installed the following Beats on these machines:
- Web1 [10.0.0.5]: Metricbeat and Filebeat
- Web2 [10.0.0.6]: Metricbeat and Filebeat
- Web3 [10.0.0.8]: Metricbeat and Filebeat

These Beats allow us to collect the following information from each machine:
-Meticbeat will allow the elk server to collect information on machine uptime, cpu usage, and other system diagnostics.
-Filebeat will allow the elk server to collect information events that happen on the machine itself such as logins and data access.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the each Yaml file into its own blank nano file with the a .yml designation these files must be located in /etc/ansible
- Copy the metircbeat-confg file into a blank nano file titled metricbeat-config.yml this file must be place in /etc/ansible
- Copy the filebeat-confg file into a blank nano file titled filebeat-config.yml this file must be place in /etc/ansible
- Update the ansiable hosts file with nano /etc/ansiable/hosts to include the following sections
 
  [webservers]
  10.0.0.5 ansible_python_interpreter=/usr/bin/python3
  10.0.0.6 ansible_python_interpreter=/usr/bin/python3
  10.0.0.8 ansible_python_interpreter=/usr/bin/python3
   
  [Elk]
  10.1.0.4 ansible_python_interpreter=/usr/bin/python3

- Ensure all virtual machines are up and running
- Run the DVWA playbook
- SSH from your ansible contanier to any of the webserver machines and run the following string:
  curl localhost/setup.php
  this should return html code
- Return to the ansible container
- Run the elk playbook
- Navigate to http://52.247.202.16/app/kibana to check that the installation worked as expected.
- On the kibana webpage click add logs, then system logs, and finaly the filebeat link under the DEB tab 
- Run the filebeat.yml playbook
- On the Kibana webpage click the blue test data button on the filebeat instruction page you opened privously. This should show a report of system event.
- On the Kibana webpage return to the add logs option. click on docker metirc and find the link for metircbeat under the deb tab.
- Run the metricbeat.yml playbook
- like with file beat click the blue test data button to check if the instalation worked.
- If all the above steps were sucessful the elk server has been properly set up and is running correctly. 
