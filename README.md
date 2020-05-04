# cyberbootcamp_UNIT13HW_WS

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

Images/Project 1 Unit 13 - network diagram.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the filebeat-playbook.yml file may be used to install only certain pieces of it, such as Filebeat.

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
A jumpbox is a node on a network typically used to access devices in a separate security zone. It also provides the ability to provision machines on a network from a single source.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system files.
Filebeat logs information about the file system, including which files have changed and when.
Metricbeat records metrics and statistics.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| ELKserver| ELK      | 10.0.0.8   | Linux            |
| DVWAVM1  | webserver| 10.0.0.5   | Linux            |
| DVWAVM2  | webserver| 10.0.0.6   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: <my home IP> ##do not wish to share publicly

Machines within the network can only be accessed by the Jumpbox.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | <my home IP>         |
| ELKserver| Yes (over port 5601)| 10.0.0.4             |
| DVWAVM1  | No                  | 10.0.0.4             |
| DVWAVM2  | No                  | 10.0.0.4             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
re-configuration can be repeated automatically.

The playbook implements the following tasks:

Runs the following command through the shell: sysctl -w vm.max_map_count=262144
This command configures the target VM (the machine being configured) to use more memory. This improves performance of the ELK container running inside.

Installs the following packages:
docker.io: The Docker engine, used for running containers.
python-pip: Package used to install Python software.
docker: Python client for Docker. Required by ELK.

Downloads the Docker container called sebp/elk.

Configures the container to start with the following port mappings:
5601:5601
9200:9200
5044:5044

Starts the container.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

Images/kibana landing page.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
10.0.0.5

We have installed the following Beats on these machines:
Filebeat

These Beats allow us to collect the following information from each machine:
Filebeat logs information about the file system, including which files have changed and when.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

- Copy the filebeat-config.yml file to /etc/filebeat/filebeat-config.yml.
- Update the filebeat-config.yml file:
Scroll to line #1106 and replace the IP address with the IP address of your ELK machine.
output.elasticsearch:
hosts: ["10.0.0.12:9200"]
username: "elastic"
password: "changeme"
Scroll to line #1806 and replace the IP address with the IP address of your ELK machine.
setup.kibana:
host: "10.0.0.12:5601"
Save this file in  /etc/ansible/files/filebeat-configuration.yml.
- Ensure the hosts file is added with the webserver private IP address that will have filebeat installed.
- Run the playbook, and navigate to /etc/filebeat/filebeat.yml to check that the installation worked as expected.
