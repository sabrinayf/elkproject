## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below:

![]Diagram/Network_Diagram.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook.yml file may be used to install only certain pieces of it, such as Filebeat.

  - Enter the playbook file._: filebeat.yml

```
--
  - name: Installing and Launch Filebeat
    hosts: elkservers
    remote_user: sabrinay
    become: yes
    tasks:
      # Use command module
    - name: Download filebeat .deb file
      command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebea$
      # Use command module
    - name: Install filebeat .deb
      command: dpkg -i filebeat-7.4.0-amd64.deb
      # Use copy module
    - name: Drop in filebeat.yml
      copy:
        src: ./filebeat-configuration.yml
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
```


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly effective, in addition to restricting access to the network.

##### What aspect of security do load balancers protect? What is the advantage of a jump box?_

* Load balancers protect by mitigating DoS attacks by distributing the traffic evenly across servers. 

* The Jumpbox machine is a gateway for us to connect to the other Virtual Machines's on the Azure Network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the files and system logs.
- What does Filebeat watch for?_
- What does Metricbeat record?_
* Filebeat collects the log data to centralize the filing system and Metricbeat collects the machine statistics and metrics such as the uptime. They both can then output the logfiles or the log events results to the location specified; to Logstash and Elasticsearch.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| DVWA-VM1 |  SSH     | 10.0.0.5   | Linux            |
| DVWA-VM2 |  SSH     | 10.0.0.7   | Linux            |
|ELK-SERVER|  SSH     | 10.0.0.8   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
52.255.145.91

Machines within the network can only be accessed by SSH.
- Which machine did you allow to access your ELK VM? What was its IP address?_ 
    + The Jumpbox machine was allowed access, IP Address: 52.255.145.91

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | No                  | 10.0.0.4             |
| ELK-VM   | No                  | 10.0.0.8             |
|          |                     |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- What is the main advantage of automating configuration with Ansible?_
* The main advantage would be the playbooks, it was very simple to set them up and automate the tasks. 

##### The playbook implements the following tasks:

* Configuring ELK VM with the Docker to the hosts files and adding me (sabrinay) as a remote user
* Installing docker to the machine
* Installing python and the python modules
* Virtual memory is also being increase, the syntax to do so is provided in the playbook. command: ``` sysctl -w vm.max_map_count=262144 ``` 
* Finally the docker container will be downloaded with image and with access to the mentioned published ports: 
``` 5601:5601,9200:9200,5044:5044 ```


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
```[webservers] 
 10.0.0.8 
```

We have installed the following Beats on these machines:
Filebeat and Metricbeat

These Beats allow us to collect the following information from each machine:
Filebeat collects the log data to centralize the filing system, it keeps all the files written to disk memory. An example would be shipping the logs files to Elastic search. Whereas, Metricbeat collects the machine statistics and server metrics such as the uptime. E.g Metricbeat analyzes and monitors memory, CPU and loads. Both of these beats can be configured to a specific output destination but usually it ships the logs/data directly to Elastic search

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

* Copy the ``` filebeat.yml ``` and the ``` metricbeat.yml ``` file to the ``` /etc/ansible directory ```
* Update the `` hosts `` file (``` nano hosts ```) to include the ELK machine under [webservers] and add the IP address of the machine (10.0.0.8).
* After running the playbook (``` ansible-playbook filebeat.yml ```) , navigate to 13.68.154.40:5601 to check that the installation worked as expected. 


