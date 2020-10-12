## Setup Docker

### Install Docker on Amazon Linux

- To setup and install docker
```
yum -y install docker nginx git java-1.8.0-openjdk java-1.8.0-openjdk-devel
yum -y remove java-1.7.0-openjdk-1.7.0.261-2.6.22.1.83.amzn1.x86_64
service docker start
usermod -a -G docker ec2-user
```

- Install Jenkins on Amazon Linux

```
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
rpm --import http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key
yum install jenkins / yum install jenkins --nogpgcheck
```

- Configure nginx /etc/nginx/nginx./conf

Add the following snippet

```
server {
    listen       80;
    server_name  _;

    location / {
            proxy_pass http://127.0.0.1:8080;
    }
}
```

- Add jenkins user to docker

```
useradd jenkins
usermod -a -G docker jenkins
```

- Setup JAVA_HOME to 
```
# Export JAVA
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk
export PATH=/usr/lib/jvm/java-1.8.0-openjdk/bin:$PATH

# User specific environment and startup programs
PATH=/usr/lib/jvm/java-1.8.0-openjdk/bin:$PATH:$HOME/bin
```

# Overwrite Java alternative /usr/bin/java
``
update-alternatives --install /usr/bin/java java /usr/lib/jvm/java-1.8.0-openjdk/bin/java 2082
``

- Start all services and Enable service after reboot

```
service docker start
service jenkins start
service nginx start
chkconfig docker on
chkconfig jenkins on
chkconfig nginx on
```

# Setup Jenkins admin pass
cat /var/lib/jenkins/secrets/initialAdminPassword
