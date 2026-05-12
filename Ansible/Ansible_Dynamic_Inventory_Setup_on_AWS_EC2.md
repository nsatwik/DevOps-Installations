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
inventory = /opt/ansible/inventory/aws_ec2.yaml
remote_user = ubuntu
private_key_file = /opt/ansible/keys/mykey.pem
host_key_checking = False
deprecation_warnings = False
```
**Set the Homepath for ansible**
```bash
vi ~/.bashrc
```
**Add this line**
```bash
export ANSIBLE_CONFIG=/opt/ansible/ansible.cfg
```
**Save and exit**
**Now check**
```bash
ansible --version
```
**You can see the path of ansible config_path=/opt/ansible**
**Now Edit file in /opt/ansible/inventory/aws_ec2.yaml**
```bash
plugin: amazon.aws.aws_ec2

regions:
  - ap-south-1

filters:
  tag:project: demo
  instance-state-name: running

compose:
  ansible_host: private_ip_address

keyed_groups:
  - key: tags.environment
    prefix: env

  - key: tags.role
    prefix: role
```
**Now use this command and all the ec2 will be seen, confirming that all are communicating**
```bash
ansible-inventory --graph
```
Finally check 
```bash
ansible -m ping all
```
