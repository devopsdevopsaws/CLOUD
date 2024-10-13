## CLASS-1
*Create your own portfolio*
```
https://www.youtube.com/watch?v=zPYVI6ciX_0&list=PLMj5OfHGyNU9_iAphUyufJngCj6kOyLFj
https://github.com/saikiranpi/LetsEncrypt_Free
```
## CLASS -2
* Under stand IP adress networking and how does it work


![IP_1](https://github.com/user-attachments/assets/45c31c63-5057-4727-a9ce-03dad04bfddc)


![IP_adress](https://github.com/user-attachments/assets/46c7e22f-b8b6-49c7-be70-27ae3aad1450)
  

## CLASS - 3
* Create vpc and create subnets and attach to internet

  
![vpc](https://github.com/user-attachments/assets/1c1bd972-bc4a-4384-8521-2bb94923ab55)

## CLASS - 4

*vpcpeering*
![vpc_peering](https://github.com/user-attachments/assets/3bceb1aa-571e-43f0-bcee-cedf914f6ab8)

## CLASS -5 

*Vpc Flow logs*

![vpc_flowlogs](https://github.com/user-attachments/assets/b77e0644-baf9-453d-bd72-7359b2b621b2)

create an instance and isntall nginx script in tjat
create a s3 bucket to store vpc flow logs
go vpc --> flowlogs --> and assign bucket 

with the help of small script push the traffic to the web application

```
while true
do 
curl <instance_dns> | grep -i nginx
sleep 1
done
```

## CLASS -6

*End points*

First achive below 
create a VPC with public and private subnet and acess the files from the s3 through Internet fom public ec2 instance and internet via natgateway from private instance

![endpoint_before](https://github.com/user-attachments/assets/14aafb4a-18b4-454b-b53c-8f4a3c249677)

The disadvantage of above is 
1. cost
2. Traffic is going via public


**ENDPOINTS**
-------------

**Gateway Endpoint**

creating gateway endpoint
endpoint --> services(gateway) --> com.amazonaws.us-east-1.s3 --> select vpc --> Select private subnet 

it will automatecally reflect in the private route table

![gateway_endpoint](https://github.com/user-attachments/assets/460f24f3-88b8-4110-acca-694a917796d1)


**Interface Endpoint**

The scenario is like in session manager you can directly able to acess the ec2 iusntances 

Interface end points
---------------------
Create a role with below permissions and assign to instances and restart the instances  then automatically you can able to see the public isntance in sessions manager after restart
```
AmazonSSMManagedInstanceCore
AmazonSSMFullAccess
```

Go to VPC --> Endpoints --> create below three different endpoints --> private subnet --> select VPC --> SG --> Restart the privare instance ..This will show the instances in session manager
```
com.amazonaws.us-east-1.ec2messages
com.amazonaws.us-east-1.ssmmessages
com.amazonaws.us-east-1.ssm
```
## CLASS -7

**Security groups vs NACL**
Stateful Firewalls (like AWS Security Groups)

How They Work: Stateful firewalls remember the state of connections. They keep track of all active connections (like a phone call where both parties know they’re talking to each other).
What This Means: If an incoming request is allowed, the response is automatically allowed. You don’t need to set extra rules for the outgoing response.
Where Used in AWS: Security Groups are stateful. They’re used to control access to specific servers (instances).


Stateless Firewalls (like AWS Network ACLs)
How They Work: Stateless firewalls treat every packet of data separately, without remembering past connections (like a security guard checking every single package delivered, without knowing the previous deliveries).
What This Means: You have to create rules for both incoming and outgoing traffic since the firewall doesn’t remember any previous traffic.
Where Used in AWS: Network ACLs (NACLs) are stateless and used to control traffic at the subnet level (the wider network area).


Main Differences
-----------------
Stateful Firewalls (Security Groups): Remember connections, need fewer rules, and are good for protecting individual servers.
Stateless Firewalls (Network ACLs): Don’t remember connections, need more rules, and are good for controlling traffic for larger parts of your network.
Think of Security Groups as a smart bouncer at a club entrance who recognizes returning guests, while Network ACLs are like airport security that checks every person every time, no matter what.

## CLASS -8*

**NAT GW**

In simple terms, a NAT Gateway helps your private network talk to the internet without letting the internet talk back directly, keeping everything inside safe and private.

![NATGW](https://github.com/user-attachments/assets/83a37330-8806-4e91-8130-1e9e77ed36be)

## CLASS - 9*
**TRANIST GW**

VPC Peering is best for small setups where only a few VPCs need to communicate directly.
Transit Gateway is ideal for larger, more complex environments where many VPCs or on-premises networks need to connect and communicate efficiently.

Go Lab to understand better 

![TGW_1](https://github.com/user-attachments/assets/193252e3-acaa-4ef3-a58f-8e8809ad45b7)
![TGW](https://github.com/user-attachments/assets/2f4326bf-f2f1-40e3-b366-87c73ea66af2)

## CLASS - 10##
**Cost-Saving Strategies and Instance Types Explained**

Instance types
```
1.OnDemand Instance
2.Reserved Instance
3.Spot Instance
4.Launch Template
```

```
#!/bin/bash
yum update -y
yum install nginx -y
service nginx start
systemctl enable nginx
echo "<h1>$(cat /etc/hostname)</h1>" >> /usr/share/nginx/html/index.html


#!/bin/bash
I=1
sgids='sg-0db879534883614a1'
for subnet in 'subnet-0afb48f410583fb34' 'subnet-0bece4d063faced1b' 'subnet-01e2d92c2669abba8'
do
    echo "Creating EC2 Instance in $subnet ..."
    aws ec2 run-instances --instance-type t2.nano --launch-template LaunchTemplateId=lt-027877668eabdf2f4 --security-group-ids $sgids --subnet-id $subnet --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=AWSB28-Server-'${I}'}]' >> /dev/null 2>&1
    echo "Created EC2 Machine with the name Testserver-${I}"
    I=$((I+1))
done
```
## CLASS - 11##
**EBS Volume**

Understanding Storage Types In AWS, storage options vary based on the type of data you're working with and your performance and durability requirements. Let's delve into the different types of storage available, focusing on EBS, which provides block-level storage volumes for use with EC2 instances.

Storage Types

Block Storage Elastic Block Storage (EBS) Volumes: Provides persistent block storage volumes that can be attached to EC2 instances. Allows you to create, attach, and detach volumes to EC2 instances as needed. Supports different volume types optimized for various workloads, including General Purpose SSD (gp2/gp3), Provisioned IOPS SSD (io1/io2), and Throughput Optimized HDD (st1). Instance Storage: Directly attached storage to EC2 instances. Provides high I/O performance but is non-persistent. Data stored in instance storage is lost if the instance is stopped or terminated. Typically available in fixed sizes and types and limited to specific instance types.

File Storage AWS Elastic File System (EFS): Fully managed file storage service that supports NFSv4 protocol. Offers scalable and highly available file storage for Linux-based workloads, allowing multiple EC2 instances to access the same file system concurrently. AWS FSx: Provides fully managed file systems optimized for Windows-based workloads, including Windows File Server and Lustre.

Object Storage Amazon Simple Storage Service (S3): Object storage service designed to store and retrieve any amount of data from anywhere on the web. Ideal for storing unstructured data, such as images, videos, documents, and backups. Offers high durability, availability, and scalability at a low cost. Amazon Glacier: Low-cost storage service designed for long-term data archiving and backup. Offers multiple retrieval options with varying latency, allowing you to optimize costs based on your access requirements. Advantages and Use Cases of EBS Volumes Permanent Storage: EBS volumes provide persistence, ensuring that data remains intact even if the associated EC2 instance is stopped or terminated. Flexible Volume Types: Choose from a variety of EBS volume types optimized for different performance and cost requirements, ranging from high-performance SSDs to cost-effective HDDs. Scalability and Attachment Flexibility: Easily scale EBS volumes up to 16TB in size and attach/detach them to different EC2 instances as needed. Practical Implementation and Best Practices Volume Provisioning and Mounting: Provision EBS volumes and mount them to EC2 instances using standard Linux commands like lsblk, fdisk, mkfs, and mount. Update the /etc/fstab file to automatically mount EBS volumes at boot time. Performance Optimization: Utilize different EBS volume types based on your application's performance requirements, ensuring optimal I/O performance and cost-effectiveness. By understanding the various storage options available in AWS, including EBS volumes, you can architect scalable and reliable storage solutions tailored to your specific workload requirements and budget constraints.

Lab follow below

```
lsblk
sudo mkfs -t ext4 /dev/xvdf
sudo mkdir /mnt/ebs-volume
sudo mount /dev/xvdf /mnt/ebs-volume
df -h
sudo blkid /dev/xvdf
sudo vi /etc/fstab
UUID=<uuid> /mnt/ebs-volume ext4 defaults,nofail 0 2

```




