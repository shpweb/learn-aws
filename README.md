Configure AWS CLI  
[How to use User data](./userdata.md)  
AWS Udemy Courses  
AWS Skill Builder  
AWS Workshop  

## DEMO
1. EC2 with normal file creation user data 
2. vpc 
3. IAM
4. S3
5. [AWS CLI for launching ec2 instance with user data](https://github.com/shpweb/learn-aws/blob/main/userdata.md#3-aws-cli-for-launching-ec2-instance-with-userdata-1) 
6. AWS CLI for filter output and terminate instance
```sh
aws ec2 describe-instances
aws ec2 describe-instances --filters Name=instance-state-name,Values=running --query "Reservations[*].Instances[*].InstanceId"
aws ec2 terminate-instances --instance-ids i-041f8983aa9bb52ae
```
7. AWS EKS (if time permits)

## Hands-on
1. as per slide (from console)
2. as per slide (from AWS CLI)
