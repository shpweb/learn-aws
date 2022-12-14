# Run commands on your Linux instance at launch

### 1. Simple userdata script
```sh
#!/bin/bash
touch /dev/hello.txt
echo "hello all" > /dev/hello.txt
```

### 2. userdata script for Apache (to install apache when launch the instance) [^1]
```sh
#!/bin/bash
yum update -y
yum install -y httpd 
systemctl start httpd
systemctl enable httpd
echo “Hello World from Hardik” > /var/www/html/index.html
```
### 3. AWS CLI for launching ec2 instance with userdata: [^1]
```sh
aws ec2 run-instances --image-id ami-076e3a557efe1aa9c --count 1 --instance-type t2.micro \
--key-name webaayu-linux-mumbai --subnet-id subnet-0c09de0366afeb08f --security-group-ids sg-09f17fec51fb75381 \
--user-data file:///Users/hardik.patel/Documents/userdata.txt
```



# How can I utilize user data to automatically run a script with every restart of my Amazon EC2 Linux instance? [^2]

By default, user data scripts and cloud-init directives run only during the first boot cycle when an EC2 instance is launched. However, you can configure your user data script and cloud-init directives with a mime multi-part file. 

- Stop the system and edit user data (instance settings) and update below: 

```sh
Content-Type: multipart/mixed; boundary="//"
MIME-Version: 1.0

--//
Content-Type: text/cloud-config; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="cloud-config.txt"

#cloud-config
cloud_final_modules:
- [scripts-user, always]

--//
Content-Type: text/x-shellscript; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="userdata.txt"


#!/bin/bash
/bin/echo " from India" >> /var/www/html/index.txt
yum update -y
yum install docker -y
docker run -d -p 8080:80 --name web nginx

--//
```


Other workaround: If you don't want to use above then you can also remove below file and edit user data to take it effect after restart. [^3]
```sh
rm /var/lib/cloud/instance/sem/config_scripts_user
```

Note: 
1. It's a best practice to create a snapshot of your instance before proceeding with the following resolution.
2. If your EC2 instance has an auto-assigned public IPv4 address, then stopping and starting the instance causes the IPv4 address to change. If you need the instance to have a static public IPv4 address, consider using an Elastic IP address. Elastic IP addresses persist after an instance stop and start.


#### References:
[^1]:[AWS Document-1](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html)  
[^2]:[AWS Document-2](https://aws.amazon.com/premiumsupport/knowledge-center/execute-user-data-ec2/)
[^3]:https://serverfault.com/questions/797482/how-to-make-ec2-user-data-script-run-again-on-startup  

