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


The disadvantage of above is 
1. cost
2. Traffic is going via public


**ENDPOINTS**
-------------

*Gateway Endpoint*

creating gateway endpoint
endpoint --> services(gateway) --> com.amazonaws.us-east-1.s3 --> select vpc --> Select private subnet 

it will automatecally reflect in the private route table



*Interface Endpoint*

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


