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
Run the command ```nodetool status``` to make sure that cluster is up and running normal
As a result you are expected to see the response that similar to the following. Pay attention to UN that means UP and NORMAL
```
Datacenter: datacenter1
=======================
Status=Up/Down
|/ State=Normal/Leaving/Joining/Moving
--  Address    Load       Tokens       Owns (effective)  Host ID                               Rack
UN  127.0.0.1  237.64 KB  256          100.0%            3acb6900-99a6-495d-99c4-652832ceb0f6  rack1
```
Now Cassandra is installed and you can start CQL (Cassandra Query Language) shell typing
```
CQLSH
```
Following option are available to produce colored output  
```
cqlsh --color
```

## Cassandra Data Model Essentials

First of all everyone who are new to Cassandra should accept that it is not Relational DBMS. There are some fundamental differences that affect the way how you use it. Cassandra is NoSQL DBMS, but it doesn't mean that you are not allowed to use SQL, rather it means you can use NOT ONLY SQL.
First of all let look to key terminology differences between most of RDBMS and Cassandra.

| RDBMS | Cassandra |
| --- | --- |
| Database | Keyspace |
| Table | Column Family |
| Primary Key | Row Key |
| Fixed Schema | Flexible Schema |

To see the list of available Keyspaces
```
DESCRIBE keyspaces;
```
To see the list of available Keyspaces with attributes (including replication factor)
```
SELECT * FROM system_schema.keyspaces;
```
To create new Keyspace (Database in terms of RDBMS) 
```
CREATE KEYSPACE my_keyspace WITH replication = {'class':'SimpleStrategy', 'replication_factor' : 3};
```
Now let create Column Family (Table). As you can see CQL is very much looks like SQL. To activate Keyspace the command ```USE my_keyspace``` can be use. In this case there is no need to use alias as I used in following snippet
```
CREATE TABLE IF NOT EXISTS my_keyspace.staff (
id int PRIMARY KEY, 
name text, 
surname text, 
email text,
bdate date
) 
```
Let now inster several records to the Column Family
```
INSERT INTO staff (id, name, surname, email, bdate) VALUES (1, 'Samir', 'Memmedov', 's@samir.az', todate(now()));
INSERT INTO staff (id, name, surname, email, bdate) VALUES (2, 'Kamil', 'Safarov', 'k@safar.az', todate('1985-03-23'));
INSERT INTO staff (id, name, surname, email, bdate) VALUES (3, 'Arzu', 'Karimlu', 'arzu@SimpleStrategy.az', todate('1989-07-19'));
INSERT INTO staff (id, name, surname, email, bdate) VALUES (4, 'Nigar', 'Aghayeva', 'nigar@strategy.az', todate('1989-09-29'));
```

