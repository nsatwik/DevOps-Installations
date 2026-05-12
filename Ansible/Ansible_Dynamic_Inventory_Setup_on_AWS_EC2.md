## Launch all necessary EC2 instances, and create tags like name, environment, role and project ##
**Go to iam and create a role for EC2 with EC2READONLYACCESS, and attach this role to controller EC2**
**Login to Controller or Master**
**Now copy the keypair which is in pem format from local to Ansible controller**
```bash
scp -i mykey.pem mykey.pem ubuntu@pub.ip:/home/ubuntu/mykey.pem
```

**Now Create a directory path**
```bash
mkdir -p /opt/ansible
cd ansible
mkdir keys inventory
touch ansible.cfg
touch inventory/aws_ec2.yml
mv /home/ubuntu/mykey.pem /opt/ansible/keys/
cd /opt/ansible/keys
chmod 400 mykey.pem
```
**Install Dependencies**
```bash
apt update
apt install python3-boto3 python3-botocore python3-pip awscli -y
apt install ansible-core -y
ansible-galaxy collection install amazon.aws
```
**Configure aws using access_keys, secret_access_keys & region which configured in iam**
```bash
aws configure
```
**Now edit ansible.cfg file**
```bash
vi /opt/ansible/ansible.cfg
```
**paste this syntax**
```bash
[defaults]
inventory = /opt/ansible/inventory/aws_ec2.yml
remote_user = ubuntu ##or root as per the requirement
private_key_file = /opt/ansible/keys/mykey.pem
host_key_checking = False
deprecation_warnings = False
```
