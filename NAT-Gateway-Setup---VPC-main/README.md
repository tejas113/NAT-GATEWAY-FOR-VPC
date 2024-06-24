# NAT-Gateway-Setup---VPC

![Image Alt Text](https://github.com/kk4977/NAT-Gateway-Setup---VPC/blob/main/NatGateway.png)

# Setting Up a VPC with Public and Private Subnets with NAT Gateway in AWS

This guide provides instructions for setting up a VPC with public and private subnets in AWS, enabling internet access from the public subnet using a NAT Gateway, and SSHing from a public instance to a private instance.

## Steps

### Step 1: Create a VPC

- Navigate to the VPC Dashboard in the AWS Management Console.
- Click on "Create VPC".
  - **Name tag**: Enter a name for your VPC (e.g., `MyVPC`).
  - **IPv4 CIDR block**: Use `12.0.0.0/16` for the entire VPC.

### Step 2: Create Subnets

#### Public Subnet (`12.0.1.0/24`)

- Navigate to "Subnets" in the VPC Dashboard.
- Click on "Create subnet".
  - **Name tag**: Enter a name for your public subnet (e.g., `PublicSubnet`).
  - **VPC**: Select the VPC created in Step 1.
  - **Availability Zone**: Choose an availability zone and Update.
  - **IPv4 CIDR block**: Use `12.0.1.0/24` for the subnet.
  - **Auto-assign public IPv4 address**: Enable this option.

#### Private Subnet (`12.0.2.0/24`)

- Navigate to "Subnets" in the VPC Dashboard.
- Click on "Create subnet".
  - **Name tag**: Enter a name for your private subnet (e.g., `PrivateSubnet`).
  - **VPC**: Select the same VPC created in Step 1.
  - **Availability Zone**: Choose the same availability zone as the public subnet.
  - **IPv4 CIDR block**: Use `12.0.2.0/24` for the subnet.
  - **Auto-assign public IPv4 address**: Disable this option (private subnet).

### Step 3: Launch Instances

#### Launch an EC2 Instance in the Private Subnet

- Navigate to "Instances" in the EC2 Dashboard.
- Click on "Launch Instance".
  - Choose an Amazon Machine Image (AMI) and instance type.
  - **Network**: Select the VPC created in Step 1.
  - **Subnet**: Choose the private subnet (`12.0.2.0/24`).
  - **Auto-assign Public IP**: Disable this option.
  - Configure storage, security groups, and key pairs as needed.

#### Launch an EC2 Instance in the Public Subnet

- Navigate to "Instances" in the EC2 Dashboard.
- Click on "Launch Instance".
  - Choose an Amazon Machine Image (AMI) and instance type.
  - **Network**: Select the VPC created in Step 1.
  - **Subnet**: Choose the public subnet (`12.0.1.0/24`).
  - **Auto-assign Public IP**: Enable this option.
  - Configure storage, security groups, and key pairs as needed.

### Step 4: Set Up a NAT Gateway for Internet Access

- Navigate to the VPC Dashboard.
- Click on "NAT Gateways" and then "Create NAT Gateway".
  - Choose the public subnet (`12.0.1.0/24`) created in Step 2.
  - Select or allocate an Elastic IP address.
  - Click "Create NAT Gateway".

### Step 5: Update Route Tables

- Navigate to "Route Tables" in the VPC Dashboard.
- Identify the route table associated with your private subnet (`12.0.2.0/24`).
- Edit the route table and add a route for destination `0.0.0.0/0` pointing to the NAT Gateway created in Step 4.

### Step 6: Verify Internet Access

- Launch an EC2 instance in the private subnet (`12.0.2.0/24`).
- SSH into the EC2 instance launched in the public subnet (`12.0.1.0/24`).
  ```bash
  ssh -i your-key.pem ec2-user@public-ip-address
- - - -![Image Alt Text](https://github.com/kk4977/NAT-Gateway-Setup---VPC/blob/main/ssh-public-instance-to-Private-instance(NatGateway).png)

- Try to ping a public domain (e.g., `ping google.com`) to verify outbound internet connectivity.

- - - -![Image Alt Text](https://github.com/kk4977/NAT-Gateway-Setup---VPC/blob/main/check-internet.png)

## Clean Up

Don't forget to clean up resources when you no longer need them to avoid unnecessary costs:

- Delete the NAT Gateway.
- Disassociate and release the Elastic IP address.
- Delete any unused subnets or route tables.
  
