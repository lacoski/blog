---
Description: ""
Keywords:
- Ceph
Section: post
Slug: ceph-solution
Tags:
- Ceph
Title: Phần 2 - Các giải pháp Ceph
Topics:
- Ceph

Url: ceph-solution
date: 2018-12-02
---

Các giải pháp của Ceph bao gồm: "Ceph Block Storage", "Ceph Filesystem", "Ceph Object Storage"

<!--more-->

## Giải pháp lưu trữ khối - Ceph block storage
Lưu trữ khối là phương thức truyền thống được sử dụng trong mạng lưu trữ (SAN). Tại đây, dữ liệu lưu trữ dưới dạng các ổ ảo nằm trên các khối, được trừ tượng hóa tại các node. Với giải pháp khối, các ổ đĩa ảo sẽ được map tới hệ điều hành, được kiểm soát bới filesystem layout. Để tạo ra giải pháp toàn diện, Ceph giới thiệu giao thức RBD - Ceph Block Device, giải pháp này cung cấp các ổ đĩa ảo tương tự với SAN. 
RBD cung cấp sự bảo đảm, tính phân phối, hiệu năng cao trên block storage disk trên mỗi client. Với bản chất các RBD block chia thành nhiều object, phân tán trên toàn Ceph Cluster, cung cấp tính bảo đảm, hiệu năng cao. RBD hỗ trợ mức Linux kernel, và được tích hợp sẵn vơi Linux kernel, cung cấp các tính năng snapshot tốc độ cao, nhẹ, copy-on-write cloning, và nhiều phương pháp nữa v.v. Hỗ trợ in-memory caching, nâng cao hiệu năng. Ceph RBD hỗ trợ image size tới 16EB. Image có thể là cung cấp dưới dạng ổ đĩa ảo, máy ảo, v.v. Các công nghệ KVM, Zen hỗ trợ đầy đủ RBD, xử lý, lưu trữ trên các VM. Ceph block hỗ trợ đầy đủ các nền tảng ảo hóa mới OpenStack, CloudStack, v.v.

![](/media/ceph-block-storage.png)

## Hệ thống tệp mới - Ceph filesystem
Hệ thống tệp của Ceph (Ceph filesystem hay CephFS) là hệ thống tệp tương thích mức POSIX, sử dụng để lưu trữ dữ liệu người dùng. CephFS hỗ trợ tốt Linux kernel driver, khiến CephFS tương thích tốt với các nền tảng Linux OS. CephFS lưu dữ liệu và các metadata riêng biệt, cung cấp hiệu năng, tính bảo đảm cho các ứng dụng nằm trên nó. Trong Ceph cluster, Cephfslib (libcephfs) chạy trên Rados library (librados) – giao thức thuộc Ceph storage - file, block, and object storage. Để sử dụng CephFS, cần ít nhất một Ceph metadata server (MDS) để chạy trong cụm. Tuy nhiên, việc sử dụng duy nhất một MDS server nó sẽ ảnh hưởng tính chịu lỗi Ceph. 
Sau khi cấu hình MDS, client có thể sử dụng CephFS theo nhiều cách. Để mount Cephfs, client cần sử dụng Linux kernel hoặc ceph-fuse (filesystem in user space) drivers cung cấp bởi cộng đồng Ceph.
Bên cạnh đó, Client có thể sử dụng phần mềm thứ 3 như Ganesha for NFS and Samba for SMB/CIFS. Phần mềm cho phép tương tác với "libcephfs", bảo đảm lưu trữ dữ liệu người dùng phân tán trong Ceph storage cluster. CephFS có thể sử dụng cho Apache Hadoop File System (HDFS). CephFS sử dụng thư viên libcephfs để lưu trữ dữ liệu tại Ceph Cluster. Thành phần libcephfs và librados rất linh hoạt và ta có thể xây dựng phiên bản tùy chỉnh, tương tác với nó, xây dựng phương pháp tổ chức dữ liệu bên dưới Ceph Storage Cluster.

![](/media/ceph-file-system.png)

## Giải pháp lưu trữ đối tượng - Ceph object storage
Phương pháp lưu trữ dữ liệu dưới dạng object thay vì trên các file, block truyền thống. Lưu trữ dạng đối tượng được coi là giải pháp lưu trữ cho nên công nghiệp lưu trữ. Vì các tổ chức mong muốn giải pháp lưu trữ toàn diện cho lượng dữ liệu khổng lồ. Từ đó Ceph xây dựng giải pháp thực sự dựa trên nền tảng lưu trữ đối tượng. Ceph phân phối tổ chức dữ liệu hoàn toàn bằng các đối tượng, cung cấp giao diện truy cập thông qua Ceph Object gateway, được biết là RADOS gateway (radosgw).
RADOS gateway (radosgw) sử dụng librgw (the RADOS gateway library) và librados, cho phép ứng dụng thiết lập kết nối với Ceph object storage. Ceph cung cấp giải pháp lưu trữ ổn định, và có thể truy cập thông qua RESTful API. The RADOS gateway cung cấp RESTful interface để sử dụng cho application lưu trữ dữ liệu trên Ceph storage cluster. 

Giao diện cung cấp bởi RADOS gồm:
- Swift compatibility: Chuẩn giao tiếp object storage của OpenStack (Swift API)
- S3 compatibility: Chuẩn giao tiếp object storage (Amazon S3 API)
- Admin API: Giao thức quản trị  (management API, native API), có thể sử dụng trực tiếp tại ứng dụng, từ đó lấy quyển truy cập vào Storage System thực hiện công tác quản trị (management purposes)

Để truy cập Ceph Object Storage System, ta có thể sử dụng RADOS gateway layer. librados software libraries cho phép user app truy cập trực tiếp đến Ceph bằng các ngôn ngữ C, C++, Java, Python, and PHP. Đồng thời, Ceph Object Storage có khả năng cung cấp khả năng lưu trữ trên nhiều site, cung cấp giải pháp khi gặp sự cố. Việc cấu hình Object Storage có thể thực hiện bởi Rados hoặc federated gateways

![](/media/ceph-object-storage.png)