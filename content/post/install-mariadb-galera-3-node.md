---
Description: ""
Keywords:
- Mysql
Section: post
Slug: install-mariadb-galera-3-node
Tags:
- Mysql
Title: Cấu hình Galera MariaDB CentOS 7
Topics:
- Mysql

Url: install-mariadb-galera-3-node
date: 2019-01-05
---

Hướng dẫn cài đặt triển khai MariaDB Galera 3 node
<!--more-->

## Mô hình

![](/media/galera-3-node.PNG)

## Chuẩn bị
### Tại Node 1
- Cấu hình Hostname

    ```
    hostnamectl set-hostname node1
    ```
- Cấu hình network

    ```
    echo "Setup IP ens160"
    nmcli c modify ens160 ipv4.addresses 10.10.10.86/24
    nmcli c modify ens160 ipv4.gateway 10.10.10.1
    nmcli c modify ens160 ipv4.dns 8.8.8.8
    nmcli c modify ens160 ipv4.method manual
    nmcli con mod ens160 connection.autoconnect yes

    echo "Setup IP ens192"
    nmcli c modify ens192 ipv4.addresses 10.10.11.86/24
    nmcli c modify ens192 ipv4.method manual
    nmcli con mod ens192 connection.autoconnect yes
    ```
- Tắt Firewall, SELinux, Khởi động lại

    ```
    sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
    sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
    systemctl stop firewalld
    systemctl disable firewalld
    init 6
    ```
- Cấu hình host

    ```
    echo "10.10.11.86 node1" >> /etc/hosts
    echo "10.10.11.87 node2" >> /etc/hosts
    echo "10.10.11.88 node3" >> /etc/hosts
    ```

### Tại Node 2
- Cấu hình Hostname

    ```
    hostnamectl set-hostname node2
    ```
- Cấu hình network

    ```
    echo "Setup IP ens160"
    nmcli c modify ens160 ipv4.addresses 10.10.10.87/24
    nmcli c modify ens160 ipv4.gateway 10.10.10.1
    nmcli c modify ens160 ipv4.dns 8.8.8.8
    nmcli c modify ens160 ipv4.method manual
    nmcli con mod ens160 connection.autoconnect yes

    echo "Setup IP ens192"
    nmcli c modify ens192 ipv4.addresses 10.10.11.87/24
    nmcli c modify ens192 ipv4.method manual
    nmcli con mod ens192 connection.autoconnect yes
    ```
- Tắt Firewall, SELinux, Khởi động lại

    ```
    sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
    sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
    systemctl stop firewalld
    systemctl disable firewalld
    init 6
    ```
- Cấu hình host

    ```
    echo "10.10.11.86 node1" >> /etc/hosts
    echo "10.10.11.87 node2" >> /etc/hosts
    echo "10.10.11.88 node3" >> /etc/hosts
    ```

### Tại Node 3
- Cấu hình Hostname

    ```
    hostnamectl set-hostname node3
    ```
- Cấu hình network

    ```
    echo "Setup IP ens160"
    nmcli c modify ens160 ipv4.addresses 10.10.10.88/24
    nmcli c modify ens160 ipv4.gateway 10.10.10.1
    nmcli c modify ens160 ipv4.dns 8.8.8.8
    nmcli c modify ens160 ipv4.method manual
    nmcli con mod ens160 connection.autoconnect yes

    echo "Setup IP ens192"
    nmcli c modify ens192 ipv4.addresses 10.10.11.88/24
    nmcli c modify ens192 ipv4.method manual
    nmcli con mod ens192 connection.autoconnect yes
    ```
- Tắt Firewall, SELinux, Khởi động lại

    ```
    sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
    sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
    systemctl stop firewalld
    systemctl disable firewalld
    init 6
    ```
- Cấu hình host

    ```
    echo "10.10.11.86 node1" >> /etc/hosts
    echo "10.10.11.87 node2" >> /etc/hosts
    echo "10.10.11.88 node3" >> /etc/hosts
    ```

## Cài đặt MariaDB (10.2)

```
Thực hiện trên tất cả các node
```

- Khai báo repo

    ```
    echo '[mariadb]
    name = MariaDB
    baseurl = http://yum.mariadb.org/10.2/centos7-amd64
    gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
    gpgcheck=1' >> /etc/yum.repos.d/MariaDB.repo
    yum -y update
    ```
- Cài đặt MariaDB

    ```
    yum install -y mariadb mariadb-server
    ```
- Cài đặt galera và gói hỗ trợ

    ```
    yum install -y galera rsync
    ```
- Tắt Mariadb

    ```
    systemctl stop mariadb
    ```

Lưu ý:
- Không bật dịch vụ mariadb sau khi cài (Liên quan tới cấu hình Galera Mariadb)


## Cấu hình Galera Cluster
### Tại `node1`
- Chỉnh sửa cấu hình

    ```
    cp /etc/my.cnf.d/server.cnf /etc/my.cnf.d/server.cnf.bak

    echo '[server]
    [mysqld]
    [galera]
    wsrep_on=ON
    wsrep_provider=/usr/lib64/galera/libgalera_smm.so
    #add your node ips here
    wsrep_cluster_address="gcomm://10.10.11.86,10.10.11.87,10.10.11.88"
    binlog_format=row
    default_storage_engine=InnoDB
    innodb_autoinc_lock_mode=2
    #Cluster name
    wsrep_cluster_name="portal_cluster"
    # Allow server to accept connections on all interfaces.
    bind-address=0.0.0.0
    # this server ip, change for each server
    wsrep_node_address="10.10.11.86"
    # this server name, change for each server
    wsrep_node_name="node1"
    wsrep_sst_method=rsync
    [embedded]
    [mariadb]
    [mariadb-10.2]' > /etc/my.cnf.d/server.cnf
    ```

Lưu ý:

- `wsrep_cluster_address`: Danh sách IP nằm trong cluster
- `wsrep_cluster_name`: Tên cluster
- `wsrep_node_address`: Ip của node chứa cấu hình
- `wsrep_node_name`: Tên node (Theo hostname)
- Không được bật mariadb (Quan trọng, nếu không sẽ dẫn tới lỗi)

### Tại `node2`
- Chỉnh sửa cấu hình

    ```
    cp /etc/my.cnf.d/server.cnf /etc/my.cnf.d/server.cnf.bak

    echo '[server]
    [mysqld]
    [galera]
    wsrep_on=ON
    wsrep_provider=/usr/lib64/galera/libgalera_smm.so
    #add your node ips here
    wsrep_cluster_address="gcomm://10.10.11.86,10.10.11.87,10.10.11.88"
    binlog_format=row
    default_storage_engine=InnoDB
    innodb_autoinc_lock_mode=2
    #Cluster name
    wsrep_cluster_name="portal_cluster"
    # Allow server to accept connections on all interfaces.
    bind-address=0.0.0.0
    # this server ip, change for each server
    wsrep_node_address="10.10.11.87"
    # this server name, change for each server
    wsrep_node_name="node2"
    wsrep_sst_method=rsync
    [embedded]
    [mariadb]
    [mariadb-10.2]' > /etc/my.cnf.d/server.cnf
    ```

### Tại `node 3`
- Chỉnh sửa cấu hình

    ```
    cp /etc/my.cnf.d/server.cnf /etc/my.cnf.d/server.cnf.bak

    echo '[server]
    [mysqld]
    [galera]
    wsrep_on=ON
    wsrep_provider=/usr/lib64/galera/libgalera_smm.so
    #add your node ips here
    wsrep_cluster_address="gcomm://10.10.11.86,10.10.11.87,10.10.11.88"
    binlog_format=row
    default_storage_engine=InnoDB
    innodb_autoinc_lock_mode=2
    #Cluster name
    wsrep_cluster_name="portal_cluster"
    # Allow server to accept connections on all interfaces.
    bind-address=0.0.0.0
    # this server ip, change for each server
    wsrep_node_address="10.10.11.88"
    # this server name, change for each server
    wsrep_node_name="node3"
    wsrep_sst_method=rsync
    [embedded]
    [mariadb]
    [mariadb-10.2]' > /etc/my.cnf.d/server.cnf
    ```

### Khởi động dịch vụ
- Tại `node1`, khởi tạo cluster

    ```
    galera_new_cluster
    systemctl start mariadb
    systemctl enable mariadb
    ```
- Tại `node2, node3`, chạy dịch vụ mariadb

    ```
    systemctl start mariadb
    systemctl enable mariadb
    ```

### Kiểm tra tại `node1`

```
[root@node1 ~]# mysql -u root -e "SHOW STATUS LIKE 'wsrep_cluster_size'"
+--------------------+-------+
| Variable_name      | Value |
+--------------------+-------+
| wsrep_cluster_size | 3     |
+--------------------+-------+
```

- Thành công

# Ý nghĩa các cấu hình

http://galeracluster.com/documentation-webpages/mysqlwsrepoptions.html