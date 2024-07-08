# DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/482ebf42-eb81-4fc4-9d26-eb41cd18a3cf)

In the Architecture diagram shown above a basic architecture of three-tier application is shown. In this diagram the first layer or tier is Presentation Layer or web tier. The second layer or tier is Application Layer or Business Tier and third layer or tier is Database Layer or Database tier. For web tier Nginx Service, for Application tier Tomcat and for Database tier MySQL and Memcache(for cache purpose) has been used as shown in the Architecture diagram above.
<br><br/>
The three tier Application is implemented using AKS as shown in the diagram below. It's Architecture diagram is same as explained above but instead of Nginx Sever, Application Gateway Ingress Controller has been used.

![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/c4d073f4-c007-41aa-add8-a8e726c96c42)

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
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/6990f16d-f80b-4148-bea8-ac2e15c3349a)
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
**Node-1**
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/0799cd86-1e32-4f25-8a26-b689db591369)
**Node-2**
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/f11288ad-bceb-442a-b07d-d33ffc8ee555)
**Node-3**
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/2f69144b-c910-415d-b539-28080f2c3ffc)
<br><br/>
Finally copy the Public IP of the Application Gateway of RabbitMQ and create the Record Set in Azure DNS Zone. Access the URL and you will see the default console for RabbitMQ, you can use the initial username and password as guest and login into the RabbitMQ console.
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/40afcab3-70e5-4cdb-abf4-ec31e0bfb704)
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/bec24e74-a2e1-4d62-883d-70e0d9c3d590)

The source code is present in Azure Repos. I have taken the Source Code present in GitHub Repository https://github.com/singhritesh85/Three-tier-WebApplication.git and did changes in pom.xml, Dockerfile, application.properties as shown below.
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/6ac43e57-0fda-4875-8cd0-209e713bd071)
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/fbc78d53-0463-4826-bb77-470a67961e64)
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/8642df42-993e-4413-93c2-b85559891020)
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/d9771bb1-b839-4b11-8808-2fab921c5119)
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/c26edc3a-4d1b-4cdc-92da-584fe6ab3179)

For Azure Artifacts, copy below section and paste to pom.xml as I have shown in above screenshot.
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/913dd4de-c185-44dc-9426-11908005cf28)
Provide Contibutor Access for Azure Artifacts as shown in screenshot below.
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/619d8baf-3add-4556-afb5-b1d5d3e66afd)
<br><br/>
Create a database named as account in mysql and import the file db_backup.sql. 
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/c069b23f-f058-4fea-9440-834fb7740133)

I have created Service service Connection for SonarQube, Azure Artifacsts and Docker Repository as shown below.
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/5e3e05ec-d8ca-482d-a74e-22adc671ff2b)

Now Run the Azure Pipeline (the azure-pipelines.yaml is used as provided with this repository). Create the URL using ingress rule for service present in the file ingress-rule.yaml in this repository. Do the entry for this URL with Public IP in Record Set of Azure DNS Zone. Access the newly created URL and provide username admin_vp and password admin_vp.
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/1884ada8-64d6-4141-bd86-f0dfdae3c610)
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/0a3a808b-55ab-428b-89ab-a37eb811c554)
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/783e3b59-40b2-413f-a126-1b386777af81)
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/345f74c8-c782-4fdd-9878-a3b5c21c5931)
When you click on the User for the first time it will get the values from MySQL Database and store it in Memcache, so that next time when you click on the same user it will provide the values from the Memcache itself.
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/36f4d33c-cbd6-462e-8899-6fe17f783a22)
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/8b3a2cad-a36a-4a98-80b8-f7be94e3fe40)

<br><br/>
After running the Azure Pipeline the Screenshot for RabbitMQ, SonarQube, Azure Artifacts are as shown in the Screenshot below.
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/faa15af6-5176-4fc7-94de-39079de9df5f)
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/d99d5211-272d-422d-a252-6a82bbe19469)
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/cb9238e9-34d4-451b-aab5-fe38f0b25679)
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/6e74fce2-838d-436c-9a9e-a10dd463876c)
![image](https://github.com/singhritesh85/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/56765895/0468da53-6ded-4290-98a8-7bc6e2f30d77)

```
kubectl create secret docker-registry devopsmelacr132827a7-auth --docker-server=https://akscontainer24registry.azurecr.io --docker-username=akscontainer24registry --docker-password=sXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXt/ -n three-tier
```
<br><br/>
<br><br/>
<br><br/>
<br><br/>
<br><br/>
```
Source Code:-  https://github.com/singhritesh85/Three-tier-WebApplication.git
```
<br><br/>
<br><br/>
<br><br/>
<br><br/>
<br><br/>
```
Reference:-  https://github.com/logicopslab/vprofile-project.git
```
