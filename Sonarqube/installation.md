**✅ STEP 1 – Prerequisites**

Update the server
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install unzip wget curl -y

```
**✅ STEP 2 – Install Java 17 (MANDATORY)**

SonarQube 9.9 requires Java 17
```bash
apt-get install openjdk-17-jdk -y
java --version
```

**----------------------------------------------------------------------------------------**

**✅ STEP 3 – Kernel Tuning (Elasticsearch WILL crash without this)**

```bash
sudo vi /etc/sysctl.conf
```
Add at bottom:

```bash
vm.max_map_count=262144
```

**✅ STEP 4 – Install PostgreSQL**

```bash
sudo apt install postgresql postgresql-contrib -y
```
Create DB + user
```bash
sudo -i -u postgres
createuser sonar
createdb sonarqube
psql
```
Inside psql
```bash
ALTER USER sonar WITH ENCRYPTED PASSWORD 'StrongPassword123';
GRANT ALL PRIVILEGES ON DATABASE sonarqube TO sonar;
\q
exit
```

**✅ STEP 5 – Download SonarQube**

First confirm architecture
```bash
uname -m
```

Must be

x86_64

Now Download
```bash
cd /opt
sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.8.100196.zip
sudo unzip sonarqube-9.9.8.100196.zip
sudo mv sonarqube-9.9.8.100196 sonarqube
```

**✅ STEP 6 – Create sonar user & fix ownership**
```bash
sudo useradd -r -M -d /opt/sonarqube -s /bin/false sonar
sudo chown -R sonar:sonar /opt/sonarqube
sudo chmod -R 755 /opt/sonarqube
```

**✅ STEP 7 – Configure DB in SonarQube**
```bash
sudo vi /opt/sonarqube/conf/sonar.properties
```
Set
```bash
sonar.jdbc.username=sonar
sonar.jdbc.password=StrongPassword123
sonar.jdbc.url=jdbc:postgresql://localhost/sonarqube
```

**✅ STEP 8 – Verify sonar.sh exists**

Run
```bash
ls -l /opt/sonarqube/bin/linux-x86-64/
```

You MUST see
```bash
sonar.sh
```
Make executable
```bash
sudo chmod +x /opt/sonarqube/bin/linux-x86-64/sonar.sh
```

**✅ STEP 9 – Create systemd service**
```bash
sudo vi /etc/systemd/system/sonarqube.service
```

Paste exactly

```bash
[Unit]
Description=SonarQube service
After=network.target postgresql.service

[Service]
Type=forking
User=sonar
Group=sonar
ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop
LimitNOFILE=65536
LimitNPROC=4096
Restart=always

[Install]
WantedBy=multi-user.target
```

**✅ STEP 10 – Register & Start**

```bash
sudo systemctl daemon-reload
sudo systemctl enable sonarqube
sudo systemctl start sonarqube
```
Check
```bash
sudo systemctl status sonarqube
```

**✅ STEP 11 – Firewall**

```bash
sudo ufw allow 9000
```

Go to security group of EC2 instance and edit inbound rules and add Custom TCP of 9000, 
because by default sonarqube runs on 9000 port. 
Now ping with public ip address of EC2 instance,
you will be redirected to login page
by default username is admin & password is also admin.
