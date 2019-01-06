---
Description: ""
Keywords:
- Openstack
Section: post
Slug: cloudinit-overview
Tags:
- Openstack
Title: Tổng quan về Cloud init và các giao đoạn thực thi
Topics:
- Openstack

Url: cloudinit-overview
date: 2019-01-01
---

Cloud init được sử dụng nhằm custom lại Linux VM trong lần boot đầu tiên

<!--more-->
## Tổng quan
Cloud init được sử dụng nhằm custom lại Linux VM trong lần boot đầu tiên

Cloud init hỗ trợ:

- Thiết lập gói cơ bản
- Cấu hình hostname
- Sinh SSH private key
- Tự động bổ sung SSH Key vào tài khoản người dùng (`.ssh/authorized_keys`)
- Cấu hình mount tự động
- Cấu hình network
- v.v

## Các giao đoạn thực thi
## Gồm 5 giai đoạn

- Generator
- Local
- Network
- Config
- Final

## 1 - Generator
Lưu ý:

- Qúa trình khởi tạo tiến trình cloud init.
- Xảy ra khi hệ thống đang boot. Quyết định có chạy dịch vụ `cloud-init.target`. Mặc định sẽ kích hoạt dịch vụ cloud init.

## 2 - Local
Lưu ý:

- Tiến trình: cloud-init-local.service
- Chạy khi mount đường dẫn '/'
- blocks: Sớm nhất khi boot, cần block network được bật

Mục đích:

- Chứa các 'local' data source
- Thực hiện cấu hình network hệ thống

Bổ sung:

- Trong nhiều trường hợp, qúa trình có thể bao gồm nhiều bước hơn bao gồm tìm kiếm data sources, quyết định network cần cấu hình.
- Khi instance được boot lần đầu, cấu hình network config sẽ được sinh. Bao gồm làm sạch các quá trình trước đó.
- Giai đoạn này sẽ block quá trình bật các card mạng, apply các cấu hình

LOG

```bash 
cloud-init[503]: Cloud-init v. 0.7.9 running 'init-local' at Fri, 30 Nov 2018 02:59:49 +0000. Up 182.97 seconds.
...
Started Initial cloud-init job (pre-networking)
...

```

## 3 - Network
Lưu ý:

- systemd service: cloud-init.service
- runs: Thực thi sau giai đoạn `local` và các cấu hình networking được bật.
- blocks: Sớm nhất khi boots
- modules: cloud_init_modules in /etc/cloud/cloud.cfg
- Yêu cầu các cấu hình network sẵn sàng, nó sẽ xử lý bất kỳ user-data này tìm thấy.

Mục đích

- Nó sẽ giải nén các module, get các dữ liệu về.
- Giai đoạn này sẽ chạy các module như `bootcmd`.

Module cho giai đoạn 3

```bash
cloud_init_modules:
 - migrator
 - bootcmd
 - write-files
 - growpart
 - resizefs
 - set_hostname
 - update_hostname
 - update_etc_hosts
 - rsyslog
 - users-groups
 - ssh
```

LOG
```
[  293.175946] cloud-init[764]: Cloud-init v. 0.7.9 running 'init' at Fri, 30 Nov 2018 03:01:36 +0000. Up 289.92 seconds.
[  296.331534] cloud-init[764]: ci-info: ++++++++++++++++++++++++++++++Net device info+++++++++++++++++++++++++++++++
[  296.346940] cloud-init[764]: ci-info: +--------+------+--------------+---------------+-------+-------------------+
[  296.471475] cloud-init[764]: ci-info: | Device |  Up  |   Address    |      Mask     | Scope |     Hw-Address    |
[  296.609673] cloud-init[764]: ci-info: +--------+------+--------------+---------------+-------+-------------------+
[  296.775927] cloud-init[764]: ci-info: |  lo:   | True |  127.0.0.1   |   255.0.0.0   |   .   |         .         |
[  296.816735] cloud-init[764]: ci-info: |  lo:   | True |      .       |       .       |   d   |         .         |
[  297.066467] cloud-init[764]: ci-info: | eth0:  | True | 10.10.11.192 | 255.255.255.0 |   .   | fa:16:3e:5d:3b:bf |
[  297.270441] cloud-init[764]: ci-info: | eth0:  | True |      .       |       .       |   d   | fa:16:3e:5d:3b:bf |
[  297.502432] cloud-init[764]: ci-info: +--------+------+--------------+---------------+-------+-------------------+
[  297.611670] cloud-init[764]: ci-info: ++++++++++++++++++++++++++++++++Route IPv4 info+++++++++++++++++++++++++++++++++
[  297.744581] cloud-init[764]: ci-info: +-------+-----------------+--------------+-----------------+-----------+-------+
[  297.884410] cloud-init[764]: ci-info: | Route |   Destination   |   Gateway    |     Genmask     | Interface | Flags |
[  298.008536] cloud-init[764]: ci-info: +-------+-----------------+--------------+-----------------+-----------+-------+
[  298.155523] cloud-init[764]: ci-info: |   0   |     0.0.0.0     |  10.10.11.1  |     0.0.0.0     |    eth0   |   UG  |
[  298.327227] cloud-init[764]: ci-info: |   1   |    10.10.11.0   |   0.0.0.0    |  255.255.255.0  |    eth0   |   U   |
[  298.412998] cloud-init[764]: ci-info: |   2   | 169.254.169.254 | 10.10.11.195 | 255.255.255.255 |    eth0   |  UGH  |
[  298.476501] cloud-init[764]: ci-info: +-------+-----------------+--------------+-----------------+-----------+-------+
```

## 4 - Config
Lưu ý:

- systemd service: cloud-config.service
- runs: After network stage.
- blocks: None.
- modules: cloud_config_modules in /etc/cloud/cloud.cfg

Mục đích:

- Giai đoạn chỉ chạy các module config. Các module không thể chạy trong giai đoạn khởi động.

```

cloud_config_modules:

 - mounts
 - locale
 - set-passwords
 - rh_subscription
 - yum-add-repo
 - package-update-upgrade-install
 - timezone
 - puppet
 - chef
 - salt-minion
 - mcollective
 - disable-ec2-metadata
 - runcmd
```

LOG
```bash
...
admin login: [  396.253652] cloud-init[990]: Cloud-init v. 0.7.9 running 'modules:config' at Fri, 30 Nov 2018 03:03:13 +0000. Up 386.17 seconds.
...
```

## 5 - Final
Lưu ý:

- systemd service: cloud-final.service
- runs: As final part of boot (traditional “rc.local”)
- blocks: None.
- modules: cloud_final_modules in /etc/cloud/cloud.cfg

Mục đích:

- Chạy sau cùng trong quá trình boot. Thực hiện các tệp lệnh cơ bản người dùng. Như `package installations`, `configuration management plugins (puppet, chef, alt-minion)`, `user-scripts (including runcmd)`.

Module

```
cloud_final_modules:
 - rightscale_userdata
 - scripts-per-once
 - scripts-per-boot
 - scripts-per-instance
 - scripts-user
 - ssh-authkey-fingerprints
 - keys-to-console
 - phone-home
 - final-message
 - power-state-change
```

LOG
```bash
[  439.819951] cloud-init[1178]: Cloud-init v. 0.7.9 running 'modules:final' at Fri, 30 Nov 2018 03:04:01 +0000. Up 434.84 seconds.
ci-info: no authorized ssh keys fingerprints found for user root.
[  440.595534] cloud-init[1178]: ci-info: no authorized ssh keys fingerprints found for user root.
ec2: 
ec2: #############################################################
ec2: -----BEGIN SSH HOST KEY FINGERPRINTS-----
ec2: 256 SHA256:9qp3+tYThbsWI+HikJomKaxZk6moSyitFWwR+JrS1KI no comment (ECDSA)
ec2: 256 SHA256:zfI5ybwOjURVMBC7vZfoitcA3JlXN+V9Y0nmD142s0c no comment (ED25519)
ec2: 2048 SHA256:1IUKHnHwJFV761UaxPh0KPjJtUABubU3NAViXGDATfc no comment (RSA)
ec2: -----END SSH HOST KEY FINGERPRINTS-----
ec2: #############################################################
-----BEGIN SSH HOST KEY KEYS-----
ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBOfwoVUOazc34REpOjQ3L+bUUGIMsodZfenVu6Vm5owTBwaGjTYSvmrwKJN1BN6R79CIWb9XGbEmMIjbVS6sJp8= 
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIFA+r8LmcwulMNZ8TjVdyGLUl9oPg5GVi+5AVcxpSq4s 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC2ZKt6h2jC1oQc0P2oWYIlBqFKv2cwTvG6kcfdgz83ygSHE/kEir7gZeM6WJ/2F804JWtdVYr95x5MAjK5rgPTv2b7Hl9FBjT5MTsxRDHrNI+Pog1U9CMrKFyDbmm0JAWttzCDG8s1I9/1CQx31GyZXKnLwv8U3CDRo3bTXG8C9//GhRySlIFVl7VeExHaueo0H02uVknRZcXbgA4ZYibryyBtPg2nw/B6db0P6eOSmrybqpJynkbtCajgsSWP2NbIRmQmzFgCwJUsrUBiH6u3UGiveFQg0xwxP+Uwzg1jxM72eqcxAsCeSRaNsNcV0K1e/CG7NExsn2/NKIqdCkz/ 
-----END SSH HOST KEY KEYS-----
[  442.791014] cloud-init[1178]: Cloud-init v. 0.7.9 finished at Fri, 30 Nov 2018 03:04:09 +0000. Datasource DataSourceOpenStack [net,ver=2].  Up 442.49 seconds
```
