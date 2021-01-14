## Setup Docker

### Install Docker on RHEL 7
We're going to use the same default docker as provided by the RHEL subscription manager.
```
subscription-manager repos \
--enable=rhel-7-server-rpms \
--enable=rhel-7-server-extras-rpms \
--enable=rhel-7-server-optional-rpms
```

Disable any docker-ce installation
```
yum-config-manager --disable docker-ce-stable
```

Installing required packages for docker
```
yum -y install lvm2 device-mapper \
device-mapper-persistent-data \
device-mapper-event \
device-mapper-libs device-mapper-event-libs
```

Now install RHEL packaged docker
```
yum install -y docker device-mapper-libs device-mapper-event-libs
```

Start all services and Enable service after reboot
```
service docker start
service nginx start
chkconfig docker on
chkconfig nginx on
```

To restart docker
```
systemctl restart docker
```

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

- Start all services and Enable service after reboot

```
service docker start
service nginx start
chkconfig docker on
chkconfig nginx on
```

## Setup Jenkins

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

### Overwrite Java alternative /usr/bin/java
``
update-alternatives --install /usr/bin/java java /usr/lib/jvm/java-1.8.0-openjdk/bin/java 2082
``

- Start all services and Enable service after reboot

```
service jenkins start
chkconfig jenkins on
```

### Setup Jenkins admin pass
cat /var/lib/jenkins/secrets/initialAdminPassword

## Install Python3
Python 2 will soon will be deprecated and major enhancement has been done for python3

```
# install python3+pip, plus optionally packages
sudo yum install python35 python35-devel python35-pip python35-setuptools python35-virtualenv

# update pip3. optionally set a symbolic link to pip3
sudo pip-3.5 install --upgrade pip
```

## Install Serverless

### Install Node (npm)

This is needed if we want to do integration with Github Actions since it uses node.js implementation
```
sudo su -
curl -sL https://rpm.nodesource.com/setup_14.x | bash -
yum -y install nodejs
```

## Install serverless via npm

Now install the serverless
```
npm install -g serverless
```
