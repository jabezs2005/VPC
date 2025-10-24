# VPC
# Aim:
To create a VPC

# Procedure:

## Create Your VPC
In this task, you will use the VPC and more option in the VPC console to create multiple resources, including a VPC, an Internet Gateway, a public subnet and a private subnet in a single Availability Zone, two route tables, and a NAT Gateway.

 

In the search box to the right of  Services, search for and choose VPC to open the VPC console.

   

Begin creating a VPC.

In the top right of the screen, verify that N. Virginia (us-east-1) is the region. 

Choose the VPC dashboard link which is towards the top left of the console.

Next, choose Create VPC. 

Note: If you do not see a button with that name, choose the Launch VPC Wizard button instead.

  

Configure the VPC details in the VPC settings panel on the left:

Choose VPC and more.

Under Name tag auto-generation, keep Auto-generate selected, however change the value from project to lab.

Keep the IPv4 CIDR block set to 10.0.0.0/16

For Number of Availability Zones, choose 1.

For Number of public subnets, keep the 1 setting.

For Number of private subnets, keep the 1 setting.

Expand the Customize subnets CIDR blocks section

Change Public subnet CIDR block in us-east-1a to 10.0.0.0/24

Change Private subnet CIDR block in us-east-1a to 10.0.1.0/24

Set NAT gateways to In 1 AZ.

Set VPC endpoints to None.

Keep both DNS hostnames and DNS resolution enabled.

 

In the Preview panel on the right, confirm the settings you have configured.

VPC: lab-vpc

Subnets:

us-east-1a

Public subnet name: lab-subnet-public1-us-east-1a

Private subnet name: lab-subnet-private1-us-east-1a

Route tables

lab-rtb-public

lab-rtb-private1-us-east-1a

Network connections

lab-igw

lab-nat-public1-us-east-1a 

 

At the bottom of the screen, choose Create VPC

The VPC resources are created. The NAT Gateway will take a few minutes to activate. 

Please wait until all the resources are created before proceding to the next step.

 

Once it is complete, choose View VPC

The wizard has provisioned a VPC with a public subnet and a private subnet in one Availability Zone with route tables for each subnet. It also created an Internet Gateway and a NAT Gateway. 

To view the settings of these resources, browse through the VPC console links that display the resource details. For example, choose Subnets to view the subnet details and choose Route tables to view the route table details. The diagram below summarizes the VPC resources you have just created and how they are configured.

### Task 1

 

An Internet gateway is a VPC resource that allows communication between EC2 instances in your VPC and the Internet. 

The lab-subnet-public1-us-east-1a public subnet has a CIDR of 10.0.0.0/24, which means that it contains all IP addresses starting with 10.0.0.x. The fact the route table associated with this public subnet routes 0.0.0.0/0 network traffic to the internet gateway is what makes it a public subnet.

A NAT Gateway, is a VPC resource used to provide internet connectivity to any EC2 instances running in private subnets in the VPC without those EC2 instances needing to have a direct connection to the internet gateway.

The  lab-subnet-private1-us-east-1a private subnet has a CIDR of 10.0.1.0/24, which means that it contains all IP addresses starting with 10.0.1.x.

 

### Task 2: Create Additional Subnets
In this task, you will create two additional subnets for the VPC in a second Availability Zone. Having subnets in multiple Availability Zones within a VPC is useful for deploying solutions that provide High Availability. 

After creating a VPC as you have already done, you can still configure it further, for example, by adding more subnets. Each subnet you create resides entirely within one Availability Zone. 

 

In the left navigation pane, choose Subnets.

First, you will create a second public subnet.

 

Choose Create subnet then configure:

VPC ID:  lab-vpc (select from the menu).

Subnet name: lab-subnet-public2

Availability Zone: Select the second Availability Zone (for example, us-east-1b)

IPv4 CIDR block: 10.0.2.0/24

The subnet will have all IP addresses starting with 10.0.2.x.

 

Choose Create subnet

The second public subnet was created. You will now create a second private subnet.

 

Choose Create subnet then configure:

VPC ID: lab-vpc

Subnet name: lab-subnet-private2

Availability Zone: Select the second Availability Zone (for example, us-east-1b)

IPv4 CIDR block: 10.0.3.0/24

The subnet will have all IP addresses starting with 10.0.3.x.

 

Choose Create subnet

The second private subnet was created. 

You will now configure this new private subnet to route internet-bound traffic to the NAT Gateway so that resources in the second private subnet are able to connect to the Internet, while still keeping the resources private. This is done by configuring a Route Table.

A route table contains a set of rules, called routes, that are used to determine where network traffic is directed. Each subnet in a VPC must be associated with a route table; the route table controls routing for the subnet.

 

In the left navigation pane, choose Route tables.

 

Select  the lab-rtb-private1-us-east-1a route table.

Note: If the newly created routes are not visible, choose refresh  button at the top to update the list of routes.

In the lower pane, choose the Routes tab.

Note that Destination 0.0.0.0/0 is set to Target nat-xxxxxxxx. This means that traffic destined for the internet (0.0.0.0/0) will be sent to the NAT Gateway. The NAT Gateway will then forward the traffic to the internet.

This route table is therefore being used to route traffic from private subnets. 

 

Choose the Subnet associations tab.

You created this route table in task 1 when you chose to create a VPC and multiple resources in the VPC. That action also created lab-subnet-private-1 and associated that subnet with this route table. 

Now that you have created another private subnet, lab-subnet-private-2, you will associate this route table with that subnet as well.

 

In the Explicit subnet associations panel, choose Edit subnet associations

 

Leave lab-subnet-private1-us-east-1a selected, but also select  lab-subnet-private2.

 

Choose Save associations

You will now configure the Route Table that is used by the Public Subnets.

 

Select the  lab-rtb-public route table (and deselect any other subnets).

 

In the lower pane, choose the Routes tab.

Note that Destination 0.0.0.0/0 is set to Target igw-xxxxxxxx, which is an Internet Gateway. This means that internet-bound traffic will be sent straight to the internet via this Internet Gateway.

You will now associate this route table to the second public subnet you created.

 

Choose the Subnet associations tab.

 

In the Explicit subnet associations area, choose Edit subnet associations

 

Leave lab-subnet-public1-us-east-1a selected, but also select  lab-subnet-public2.

 

Choose Save associations

Your VPC now has public and private subnets configured in two Availability Zones. The route tables you created in task 1 have also been updated to route network traffic for the two new subnets.

Task 2

 

### Task 3: Create a VPC Security Group
In this task, you will create a VPC security group, which acts as a virtual firewall. When you launch an instance, you associate one or more security groups with the instance. You can add rules to each security group that allow traffic to or from its associated instances.

 

In the left navigation pane, choose Security groups.

 

Choose Create security group and then configure:

Security group name: Web Security Group

Description: Enable HTTP access

VPC: choose the X to remove the currently selected VPC, then from the drop down list choose lab-vpc

 

In the Inbound rules pane, choose Add rule

 

Configure the following settings:

Type: HTTP

Source: Anywhere-IPv4

Description: Permit web requests

 

  
Scroll to the bottom of the page and choose Create security group

You will use this security group in the next task when launching an Amazon EC2 instance.

# OUTPUT:
<img width="2558" height="1599" alt="Screenshot 2025-10-24 133748" src="https://github.com/user-attachments/assets/0d25335a-43d5-4db5-aef8-4513c292d5a2" />

<img width="2559" height="1599" alt="Screenshot 2025-10-24 133930" src="https://github.com/user-attachments/assets/68930050-0fa2-49ea-b502-215618d69f6c" />

<img width="2559" height="1599" alt="Screenshot 2025-10-24 134015" src="https://github.com/user-attachments/assets/b1ab2238-1ace-4b35-9a99-e00cd9524d37" />

<img width="2559" height="1599" alt="Screenshot 2025-10-24 134210" src="https://github.com/user-attachments/assets/9ec9d58a-c7b6-4923-92dd-9f355cd06a30" />

<img width="2559" height="1599" alt="Screenshot 2025-10-24 134335" src="https://github.com/user-attachments/assets/b53d67ea-6357-444e-99d7-321fa88b64b0" />

<img width="2559" height="1599" alt="Screenshot 2025-10-24 134525" src="https://github.com/user-attachments/assets/cf209221-cbac-427d-a9ce-44a06c6a51bf" />

<img width="2559" height="1599" alt="Screenshot 2025-10-24 134622" src="https://github.com/user-attachments/assets/f3fd1ead-8c2f-40e4-bf01-2b71d4a632be" />

<img width="2559" height="1599" alt="Screenshot 2025-10-24 134910" src="https://github.com/user-attachments/assets/e5aaa40e-dfc1-4e39-b04a-f0ee65083af8" />

<img width="2559" height="1599" alt="Screenshot 2025-10-24 134919" src="https://github.com/user-attachments/assets/d87f242e-ce7e-441b-86ee-d89c74c7995c" />

<img width="2559" height="1599" alt="Screenshot 2025-10-24 135521" src="https://github.com/user-attachments/assets/6b1b3f71-7105-42eb-a5de-6543f0482f2b" />

<img width="2559" height="1599" alt="Screenshot 2025-10-24 135914" src="https://github.com/user-attachments/assets/bd2e7041-9790-423b-90bd-9766b27e24a7" />

# RESULT:
The VPC is successfully created.
