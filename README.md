In this blog you will see how the configuration the Hadoop cluster by the Ansible.

Let’s See First what is ansible and Hadoop.

# What is Hadoop?

Hadoop is an open source, a Java-based programming framework that supports the storage and processing of extremely large data sets in a distributed computing environment. It is part of the Apache project sponsored by the Apache Software Foundation.

# What is Ansible?

Ansible is an open-source software that automates software provisioning, configuration management, and application deployment.

Now Let’s start practical :

# Steps To configuratio Hadoop by ansible …

**Step 1**:Copying the hadoop software

**Step 2**: Copying the java software

**Step 3**: Installing thee hadoop software

**Step 4**: Installing the java Softwrae

**Step 5**: Create the Directory

**Step 6**: Configuration the core-site.xml

**Step 7**: Cofiguration the hdfs-site.xml

**Step 18**: Start datanode or namenode if namenode then format

First write inventory :

![image](https://user-images.githubusercontent.com/70094412/111644925-4accc280-8826-11eb-9dd5-dbcc8ca8ae70.png)

# Now Cofiguration in Namenode by hadoop.yml

hadoop.yml for Namenode

namenode ip is 192.168.0.113

datanode ip is 192.168.0.108

Namenode Confi.. : yml File..

a hadoop.yml
```
- hosts: namenode
  tasks:
          - name: "Copying the hadoop software"
            copy:
                    dest: "/root/"
                    src: "/root/hadoop-1.2.1-1.x86_64.rpm"

          - name: "Copying the jdk software"
            copy:
                    dest: "/root/"
                    src: "/root/jdk-8u171-linux-x64.rpm"
          - block:

            - name: "installing java..."
              command: "rpm -q jdk1.8"
              register: x
              changed_when: false

            rescue:
            - command: "rpm -i jdk-8u171-linux-x64.rpm"
              when: x.rc != 0
          - block:

            - name: "Installing the hadoop..."
              command: "rpm -q hadoop"
              register: y
              changed_when: false


            rescue:
            - command: "rpm -i hadoop-1.2.1-1.x86_64.rpm --force"
              when: y.rc != 0

          - name: "creating directory ..."
            file:
                    state: directory
                    path: "/nn1"

          - name: "Configration of namenode of core file ..."
            copy:
                    dest: "/etc/hadoop/core-site.xml"
                    src: "/root/all_hadoop_config_file_byansible/namenode/core-site.xml"

          - name: "Configration of namenode of hdfs file ..."
            copy:
                    dest: "/etc/hadoop/hdfs-site.xml"
                    src: "/root/all_hadoop_config_file_byansible/namenode/hdfs-site.xml"

          - name: "namenode formatiing ..."
            shell: "echo Y | hadoop namenode -format"
            ignore_errors: yes


          - name: "namenode starting ..."
            command: "hadoop-daemon.sh start namenode"
            ignore_errors: yes
           
```

``` core-site.xml  (for namenode)
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<!-- Put site-specific property overrides in this file. -->

<configuration>
<property>
<name>fs.default.name</name>
<value>hdfs://0.0.0.0:9001</value>
</property>
</configuration>
```

hdfs-site.yml (for namenode)
```
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<!-- Put site-specific property overrides in this file. -->

<configuration>
<property>
<name>dfs.name.dir</name>
<value>/nn1</value>
</property>
</configuration>
```

Output:

![image](https://user-images.githubusercontent.com/70094412/111646077-5a98d680-8827-11eb-8b80-c286da8bd160.png)

So now our Namenode is started.. see by ‘jps’ command

# Now Configuration in Datanode

hadoop2.yml for Datanode

Datanode Confi .. yml File :

a hadoop2.yml(for data node)
```
- hosts: datanode
  tasks:
          - name: "Copying the hadoop software"
            copy:
                    dest: "/root/"
                    src: "/root/hadoop-1.2.1-1.x86_64.rpm"

          - name: "Copying the jdk software"
            copy:
                    dest: "/root/"
                    src: "/root/jdk-8u171-linux-x64.rpm"
          - block:

            - name: "installing java..."
              command: "rpm -q jdk1.8"
              register: x
              changed_when: false

            rescue:
            - command: "rpm -i jdk-8u171-linux-x64.rpm"
              when: x.rc != 0
          - block:

            - name: "Installing the hadoop..."
              command: "rpm -q hadoop"
              register: y
              changed_when: false


            rescue:
            - command: "rpm -i hadoop-1.2.1-1.x86_64.rpm --force"
              when: y.rc != 0

          - name: "create the directory ..."
            file:
                    state: directory
                    path: "/dn1"
          - name: "Configration of datanode of core file ..."
            copy:
                    dest: "/etc/hadoop/core-site.xml"
                    src: "/root/all_hadoop_config_file_byansible/datanode/core-site.xml"

          - name: "Configration of datanode of hdfs file ..."
            copy:
                    dest: "/etc/hadoop/hdfs-site.xml"
                    src: "/root/all_hadoop_config_file_byansible/datanode/hdfs-site.xml"

          - name: "datanide  starting ..."
            command: "hadoop-daemon.sh start datanode"
            ignore_errors: yes
```

core-site.xml (for data node)
```
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<!-- Put site-specific property overrides in this file. -->

<configuration>
<property>
<name>fs.default.name</name>
<value>hdfs://192.168.0.113:9001</value>
</property>
</configuration>
```

hdfs-site.xml (for data node)
```
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<!-- Put site-specific property overrides in this file. -->

<configuration>
<property>
<name>dfs.name.dir</name>
<value>/dn1</value>
</property>
</configuration>
```

Output:

![image](https://user-images.githubusercontent.com/70094412/111646517-bb281380-8827-11eb-9bbb-9317611ccad0.png)

So now Our datnode is started succesfully , see by ‘jps ’ command

Now you can confirm by Type url:192.168.0.113:50070


>Thank You




