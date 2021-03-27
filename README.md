# Install and set up Apache Cassandra on CentOS

Update your OS

```yum -y update```

Check Java version in your system and update if needed 
```java -version```

Create (or update) Repository for installation of Cassandra

```vi /etc/yum.repos.d/cassandra.repo```

make sure that content of Repo is as following

```
[cassandra]
name=Apache Cassandra
baseurl=https://www.apache.org/dist/cassandra/redhat/30x/
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://www.apache.org/dist/cassandra/KEYS
```
Change ```baseurl``` in accordance with version you want to install. In my case, the choice is version 30x. Visit the URL [Apache Cassandra](https://www.apache.org/dist/cassandra/redhat/) to see available alternative versions.

To start installation

```sudo yum -y install cassandra```

Reload your system daemons by running to make sure that your system is using latest daemons:

```sudo systemctl daemon-reload```

Enable Cassandra service to automatically run after each restrat and start service
```
sudo systemctl enable cassandra
systemctl start cassandra
```
Check the status of service to make sure that it started and running
```
systemctl start cassandra
```

