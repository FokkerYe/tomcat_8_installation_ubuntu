# tomcat_8_installation_ubuntu

## Java Installation Guide

```bash
sudo apt update -y
sudo apt install openjdk-8-jdk -y
sudo apt install openjdk-11-jdk -y
```

## After running the command, select the appropriate Java version
```bash
sudo update-alternatives --config java
```
# Tomcat 8 Installation Guide
# Download and Extract Tomcat
```bash
export VER="8.5.64"
wget https://archive.apache.org/dist/tomcat/tomcat-8/v${VER}/bin/apache-tomcat-${VER}.tar.gz
tar xvf apache-tomcat-${VER}.tar.gz -C /usr/share/
```
# Create Tomcat User
```bash
useradd tomcat
passwd tomcat
```
# Set Permissions
```bash
mkdir /usr/share/tomcat
chown -R tomcat:tomcat /usr/share/tomcat
export VER="8.5.64"
chown -R tomcat:tomcat /usr/share/apache-tomcat-$VER/

```
# Create Systemd Service File
```bash
vi /etc/systemd/system/tomcat.service

```
# Add the Following Configuration to the File
```bash
[Unit]
Description=Tomcat 8 servlet container
After=network.target

[Service]
Type=forking
User=tomcat
Group=tomcat

Environment="JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64"
Environment="CATALINA_BASE=/usr/share/apache-tomcat-8.5.64"
Environment="CATALINA_HOME=/usr/share/apache-tomcat-8.5.64"
Environment="CATALINA_OPTS=-Xms2048M -Xmx2048M -server -XX:+UseParallelGC"

ExecStart=/usr/share/apache-tomcat-8.5.64/bin/startup.sh
ExecStop=/usr/share/apache-tomcat-8.5.64/bin/shutdown.sh

[Install]
WantedBy=multi-user.target

```
 ## Managing Tomcat Service
```bash
systemctl daemon-reload
systemctl enable tomcat.service                                                                                  
systemctl start tomcat.service
systemctl status tomcat.service
```
It looks like you installed Tomcat and created the tomcat user, but you're having issues with the login shell of the tomcat user. If you want the tomcat user to use the Bash shell, you need to make sure the user's shell is set to /bin/bash.

Here's how you can ensure the tomcat user is configured correctly:
Check the Current Shell for the tomcat User:
Verify the current shell for the tomcat user by inspecting the /etc/passwd file.

```

grep tomcat /etc/passwd
```

You should see something like this:

plaintext
```
tomcat:x:1001:1001::/home/tomcat:/bin/bash
```
If the shell is not /bin/bash, you need to change it.

Change the Shell for the tomcat User:
Use the chsh command to change the shell to /bin/bash.

sh

```
sudo chsh -s /bin/bash tomcat
```



