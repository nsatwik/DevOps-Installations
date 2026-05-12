## Launch all necessary EC2 instances, and create tags like name, environment, role and project ##
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
ansible-galaxy collection install amazon.aws
``
