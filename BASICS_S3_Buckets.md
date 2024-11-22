# Use the following command to list the S3 buckets in the AWS account
aws s3 ls

~ on ☁️  (us-east-1) ➜  aws s3 ls 
2024-08-20 11:29:01 kodekloud-bucket-17241533400687805496185
2024-08-20 11:29:03 kodekloud-random-172415334213351139819879

# An S3 bucket with the prefix kodekloud-bucket- contains some files. Use the AWS CLI to copy these files from the bucket to /root of your lab machine.

# First, make sure you have the full bucket name. Let’s assume the full bucket name is kodekloud-bucket-17241533400687805496185
# To copy all files from the bucket to your local machine, you can use the following command.

aws s3 cp s3://kodekloud-bucket-17241533400687805496185/ /root/ --recursive

~ on ☁️  (us-east-1) ➜  aws s3 cp s3://kodekloud-bucket-17241533400687805496185/ /root/ --recursive
download: s3://kodekloud-bucket-17241533400687805496185/test1.txt to ./test1.txt
download: s3://kodekloud-bucket-17241533400687805496185/test2.txt to ./test2.txt


