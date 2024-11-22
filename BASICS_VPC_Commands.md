

# To list the Virtual Private Clouds (VPCs) in your AWS account using the AWS Command Line Interface (CLI), you can use the following command:

aws ec2 describe-vpcs

~ on ☁️  (us-east-1) ➜  aws ec2 describe-vpcs
{
    "Vpcs": [
        {
            "CidrBlock": "192.168.0.0/16",
            "DhcpOptionsId": "dopt-03f3a2cd321b10625",
            "State": "available",
            "VpcId": "vpc-0cb7c0d75e951ee46",
            "OwnerId": "905418031803",
            "InstanceTenancy": "default",
            "CidrBlockAssociationSet": [
                {
                    "AssociationId": "vpc-cidr-assoc-00de123273b417495",
                    "CidrBlock": "192.168.0.0/16",
                    "CidrBlockState": {
                        "State": "associated"
                    }
                }
            ],
            "IsDefault": false,
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "vpc-ec2"
                }
            ]
        },
        {
            "CidrBlock": "172.31.0.0/16",
            "DhcpOptionsId": "dopt-03f3a2cd321b10625",
            "State": "available",
            "VpcId": "vpc-0999782341e83467f",
            "OwnerId": "905418031803",
            "InstanceTenancy": "default",
            "CidrBlockAssociationSet": [
                {
                    "AssociationId": "vpc-cidr-assoc-09e3367b62e9d4dc8",
                    "CidrBlock": "172.31.0.0/16",
                    "CidrBlockState": {
                        "State": "associated"
                    }
                }
            ],
            "IsDefault": true
        }
    ]
}

~ on ☁️  (us-east-1) ➜  

# For a more human-readable output, such as just listing the VPC IDs, you can use the --query option to filter the output:

aws ec2 describe-vpcs --query 'Vpcs[*].VpcId' --output text


~ on ☁️  (us-east-1) ➜  aws ec2 describe-vpcs --query 'Vpcs[*].VpcId' --output text
vpc-0cb7c0d75e951ee46   vpc-0999782341e83467f






https://docs.aws.amazon.com/vpc/latest/userguide/aws-ip-ranges.html

https://repost.aws/questions/QUWHVSZni3RS6zCOD7CLUF2g/ec2-instance-connect-ssh-source-ip

