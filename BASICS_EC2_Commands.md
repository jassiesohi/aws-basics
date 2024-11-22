
# The command below will create the key pair and save the private key directly to a file:

aws ec2 create-key-pair --key-name kk-user --query 'KeyMaterial' --output text > kk-user.pem

~ on ☁️  (us-east-1) ✖ aws ec2 create-key-pair --key-name kk-user --query 'KeyMaterial' --output text > kk-user.pem

# To specifically find the security group named kk-ec2-sg, you can add a filter to the command to only return the relevant details:

aws ec2 describe-security-groups --filters Name=group-name,Values=kk-ec2-sg --query 'SecurityGroups[*].{ID:GroupId, Name:GroupName}' --output text

# This will give you the ID of the security group named kk-ec2-sg, which you can then note for use in configuring your EC2 instances.

~ on ☁️  (us-east-1) ✖ aws ec2 describe-security-groups --filters Name=group-name,Values=kk-ec2-sg --query 'SecurityGroups[*].{ID:GroupId,Name:GroupName}' --output text
sg-05eb36246a0e561c2    kk-ec2-sg

# Create an EC2 instance using the AWS CLI with these specifications:

aws ec2 run-instances \
--image-id ami-04e5276ebb8451442 \
--count 1 \
--instance-type t2.micro \
--key-name kk-user \
--security-group-ids sg-05eb36246a0e561c2 \
--subnet-id subnet-07e17dccbf23abf10 \
--tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=kodekloud-ec2}]'


on ☁️  (us-east-1) ✖ aws ec2 run-instances \
--image-id ami-04e5276ebb8451442 \
--count 1 \
--instance-type t2.micro \
--key-name kk-user \
--security-group-ids sg-05eb36246a0e561c2 \
--subnet-id subnet-07e17dccbf23abf10 \
--tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=kodekloud-ec2}]'
{
    "Groups": [],
    "Instances": [
        {
            "AmiLaunchIndex": 0,
            "ImageId": "ami-04e5276ebb8451442",
            "InstanceId": "i-0cb283dc5577fc33f",
            "InstanceType": "t2.micro",
            "KeyName": "kk-user",
            "LaunchTime": "2024-08-20T12:00:22.000Z",
            "Monitoring": {
                "State": "disabled"
            },
            "Placement": {
                "AvailabilityZone": "us-east-1a",
                "GroupName": "",
                "Tenancy": "default"
            },
            "PrivateDnsName": "ip-192-168-0-227.ec2.internal",
            "PrivateIpAddress": "192.168.0.227",
            "ProductCodes": [],
            "PublicDnsName": "",
            "State": {
                "Code": 0,
                "Name": "pending"
            },
            "StateTransitionReason": "",
            "SubnetId": "subnet-07e17dccbf23abf10",
            "VpcId": "vpc-0cb7c0d75e951ee46",
            "Architecture": "x86_64",
            "BlockDeviceMappings": [],
            "ClientToken": "dc768015-0a2d-44a7-b051-65892450179c",
            "EbsOptimized": false,
            "EnaSupport": true,
            "Hypervisor": "xen",
            "NetworkInterfaces": [
                {
                    "Attachment": {
                        "AttachTime": "2024-08-20T12:00:22.000Z",
                        "AttachmentId": "eni-attach-03acf220f7687a0b1",
                        "DeleteOnTermination": true,
                        "DeviceIndex": 0,
                        "Status": "attaching",
                        "NetworkCardIndex": 0
                    },
                    "Description": "",
                    "Groups": [
                        {
                            "GroupName": "kk-ec2-sg",
                            "GroupId": "sg-05eb36246a0e561c2"
                        }
                    ],
                    "Ipv6Addresses": [],
                    "MacAddress": "12:0c:66:ab:f1:27",
                    "NetworkInterfaceId": "eni-0a847bb1697a7e79a",
                    "OwnerId": "905418031803",
                    "PrivateIpAddress": "192.168.0.227",
                    "PrivateIpAddresses": [
                        {
                            "Primary": true,
                            "PrivateIpAddress": "192.168.0.227"
                        }
                    ],
                    "SourceDestCheck": true,
                    "Status": "in-use",
                    "SubnetId": "subnet-07e17dccbf23abf10",
                    "VpcId": "vpc-0cb7c0d75e951ee46",
                    "InterfaceType": "interface"
                }
            ],
            "RootDeviceName": "/dev/xvda",
            "RootDeviceType": "ebs",
            "SecurityGroups": [
                {
                    "GroupName": "kk-ec2-sg",
                    "GroupId": "sg-05eb36246a0e561c2"
                }
            ],
            "SourceDestCheck": true,
            "StateReason": {
                "Code": "pending",
                "Message": "pending"
            },
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "kodekloud-ec2"
                }
            ],
            "VirtualizationType": "hvm",
            "CpuOptions": {
                "CoreCount": 1,
                "ThreadsPerCore": 1
            },
            "CapacityReservationSpecification": {
                "CapacityReservationPreference": "open"
            },
            "MetadataOptions": {
                "State": "pending",
                "HttpTokens": "required",
                "HttpPutResponseHopLimit": 2,
                "HttpEndpoint": "enabled",
                "HttpProtocolIpv6": "disabled",
                "InstanceMetadataTags": "disabled"
            },
            "EnclaveOptions": {
                "Enabled": false
            },
            "BootMode": "uefi-preferred",
            "PrivateDnsNameOptions": {
                "HostnameType": "ip-name",
                "EnableResourceNameDnsARecord": false,
                "EnableResourceNameDnsAAAARecord": false
            },
            "MaintenanceOptions": {
                "AutoRecovery": "default"
            },
            "CurrentInstanceBootMode": "legacy-bios"
        }
    ],
    "OwnerId": "905418031803",
    "ReservationId": "r-07390019b62a5592d"
}

# To delete an EC2 instance named kodekloud-ec2 that was created earlier, you will need to terminate the instance using the AWS CLI. Here’s how you can do this step by step.

# Step 1: Identify the Instance ID
# First, you need to find the instance ID of the instance named kodekloud-ec2. Use the following AWS CLI command to list the instances and filter by the tag name:

aws ec2 describe-instances --filters "Name=tag:Name,Values=kodekloud-ec2" --query "Reservations[*].Instances[*].InstanceId" --output text


~ on ☁️  (us-east-1) ➜  aws ec2 describe-instances --filters "Name=tag:Name,Values=kodekloud-ec2" --query "Reservations[*].Instances[*].InstanceId" --output text
i-0cb283dc5577fc33f


# Step 2: Terminate the Instance
# Once you have the instance ID, you can terminate the instance using the following command. Replace instance-id with the actual ID you found from the previous step:

aws ec2 terminate-instances --instance-ids instance-id

aws ec2 terminate-instances --instance-ids i-0cb283dc5577fc33f

~ on ☁️  (us-east-1) ➜  aws ec2 terminate-instances --instance-ids i-0cb283dc5577fc33f
{
    "TerminatingInstances": [
        {
            "CurrentState": {
                "Code": 32,
                "Name": "shutting-down"
            },
            "InstanceId": "i-0cb283dc5577fc33f",
            "PreviousState": {
                "Code": 16,
                "Name": "running"
            }
        }
    ]
}

~ on ☁️  (us-east-1) ➜  


