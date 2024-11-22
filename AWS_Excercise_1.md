

# (prompt) List down the step by step commands to create a new vpc with a cidr block of 10.10.0.0/16, subnet with a cidr block of 10.10.1.0/24, security group that allows SSH access, and launch an ec2 instance with a key pair with Amazon Linux 2023 with type t2.micro in us-east-1 region. 


To create a new VPC, subnet, security group, and launch an EC2 instance in the AWS CLI, follow the steps below. These commands assume you're working in the `us-east-1` region, and AWS CLI is configured with appropriate IAM permissions.

### Step 1: Create a New VPC
Create a VPC with a CIDR block of `10.10.0.0/16`:


aws ec2 create-vpc --cidr-block 10.10.0.0/16 --region us-east-1


This command will return a `VpcId` (e.g., `vpc-xxxxxxxx`). Take note of it, as it will be needed in the following steps.

### Step 2: Create a Subnet
Create a subnet within the VPC with a CIDR block of `10.10.1.0/24`:


aws ec2 create-subnet --vpc-id <VpcId> --cidr-block 10.10.1.0/24 --region us-east-1


This command will return a `SubnetId` (e.g., `subnet-xxxxxxxx`). Take note of this ID as well.

### Step 3: Create an Internet Gateway (Optional but typically needed)
Attach an Internet Gateway to the VPC to allow internet access:


aws ec2 create-internet-gateway --region us-east-1


This returns an `InternetGatewayId` (e.g., `igw-xxxxxxxx`). Attach it to your VPC:


aws ec2 attach-internet-gateway --vpc-id <VpcId> --internet-gateway-id <InternetGatewayId> --region us-east-1


### Step 4: Create a Route Table and Associate with the Subnet
Create a route table for the VPC:


aws ec2 create-route-table --vpc-id <VpcId> --region us-east-1


This returns a `RouteTableId` (e.g., `rtb-xxxxxxxx`). Create a route in the route table that directs traffic to the Internet Gateway:


aws ec2 create-route --route-table-id <RouteTableId> --destination-cidr-block 0.0.0.0/0 --gateway-id <InternetGatewayId> --region us-east-1


Associate the route table with the subnet:


aws ec2 associate-route-table --subnet-id <SubnetId> --route-table-id <RouteTableId> --region us-east-1


### Step 5: Modify Subnet to Enable Auto-Assign Public IPs
Modify the subnet to automatically assign public IP addresses to instances:


aws ec2 modify-subnet-attribute --subnet-id <SubnetId> --map-public-ip-on-launch --region us-east-1


### Step 6: Create a Security Group
Create a security group that allows SSH access (port 22):


aws ec2 create-security-group --group-name MySecurityGroup --description "Security group for SSH access" --vpc-id <VpcId> --region us-east-1


This returns a `GroupId` (e.g., `sg-xxxxxxxx`). Allow SSH access from anywhere (0.0.0.0/0):


aws ec2 authorize-security-group-ingress --group-id <GroupId> --protocol tcp --port 22 --cidr 0.0.0.0/0 --region us-east-1


### Step 7: Create a Key Pair
Create a key pair to SSH into the instance:


aws ec2 create-key-pair --key-name MyKeyPair --query 'KeyMaterial' --output text > MyKeyPair.pem --region us-east-1


Ensure the private key file has the correct permissions:


chmod 400 MyKeyPair.pem


### Step 8: Launch an EC2 Instance
Launch an Amazon Linux 2023 EC2 instance of type `t2.micro` in the created subnet:


aws ec2 run-instances --image-id resolve:ssm:/aws/service/ami-amazon-linux-latest/al2023-ami-kernel-6.1-x86_64 --count 1 --instance-type t2.micro --key-name MyKeyPair --security-group-ids <GroupId> --subnet-id <SubnetId> --region us-east-1


This command returns details about the launched instance, including the `InstanceId`.

### Summary of Parameters to Replace:
- `<VpcId>`: The VPC ID returned from the `create-vpc` command.
- `<SubnetId>`: The Subnet ID returned from the `create-subnet` command.
- `<InternetGatewayId>`: The Internet Gateway ID returned from the `create-internet-gateway` command.
- `<RouteTableId>`: The Route Table ID returned from the `create-route-table` command.
- `<GroupId>`: The Security Group ID returned from the `create-security-group` command.

These commands will create a complete setup with a VPC, subnet, security group, and an EC2 instance.