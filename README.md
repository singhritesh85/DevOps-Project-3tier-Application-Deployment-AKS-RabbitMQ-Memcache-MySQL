# DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/482ebf42-eb81-4fc4-9d26-eb41cd18a3cf)

In the Architecture diagram shown above a basic architecture of three-tier application is shown. In this diagram the first layer or tier is Presentation Layer or web tier. The second layer or tier is Application Layer or Business Tier and third layer or tier is Database Layer or Database tier. For web tier Nginx Service, for Application tier Tomcat and for Database tier MySQL and Memcache(for cache purpose) has been used as shown in the Architecture diagram above.
<br><br/>
The three tier Application is implemented using AKS as shown in the diagram below. It's Architecture diagram is same as explained above but instead of Nginx Sever, Application Gateway Ingress Controller has been used.

![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/dfb02adb-020a-430d-a106-f6086bf1ce95)

I have used terraform script as provided in this repository to create the Azure VMs, Application Gateways, AKS and Azure Database for MySQL Flexible Servers.

I have created RabbitMQ cluster with three Azure VMs. Follow below procedures to achieve the same.

Using the above terraform script and the bootstrap script used with terraform three Azure VMs for RabbitMQ will be created along with the Application Gateway.
The Bootstrapping script will install RabbitMQ on three Azure VMs. Then list the plugins, enable rabbitmq_management plugin and restart the rabbitmq-server on the three Azure VMs. To achieve this I have used below commands.
```
(a) rabbitmq-plugins list
(b) rabbitmq-plugins enable rabbitmq_management
(c) systemctl restart rabbitmq-server
Finally you can ensure that rabbitmq_management plugin is enabled or not using the command rabbitmq-plugins list
```
**First Node, Consider it as Node-1**
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/35fd035e-0d1b-400b-8f98-ba3a025851e3)
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/022de284-4490-493a-a4f4-440e7acf5e3f)
```
Run below command on Node-1 to set the policy for High Availability (HA) in RabbitMQ Cluster.
rabbitmqctl set_policy ha-all ".*" '{"ha-mode":"all","ha-sync-mode":"automatic"}'
```
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/796a981b-442c-42f1-8826-e36893ee25ef)
**Second Node, Consider it as Node-2**
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/32b7736d-0c85-4a74-a67d-b6d04e4afb77)
**Third Node, Consider it as Node-3**
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/28f8d608-5f77-47a3-a935-d625219a93cd)

To create RabbitMQ cluster of three nodes follow the below procedures.
```
On Node-1 (rabbitmq-vm-1) open the file /var/lib/rabbitmq/.erlang.cookie using cat command and copy the hash value of cookie and paste it on Node-2 and Node-3 in the file /var/lib/rabbitmq/.erlang.cookie.
Then restart rabbitmq-server service, then stop rabbitmq application using the command rabbitmqctl stop_app on Node-2 (rabbitmq-vm-2) and Node-3 (rabbitmq-vm-3). Finally run the command rabbitmqctl join_cluster rabbit@<IP_Address_Node1> and start the rabbitmq application using the command rabbitmqctl start_app on Node-2 and Node-3.
```
