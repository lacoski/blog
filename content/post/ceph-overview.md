---
Description: ""
Keywords:
- Ceph
Section: post
Slug: ceph-overview
Tags:
- Ceph
Title: Phần 1 - Tổng quan về Ceph
Topics:
- Ceph

Url: ceph-overview
date: 2018-12-01
---

Ceph là dự án mã nguồn mở, cung cấp giải pháp lưu trữ dữ liệu. Ceph cung cấp giải pháp lưu trữ phân tán mạnh mẽ, đáp ứng các yêu cầu về khả năng mở rộng, hiệu năng, khả năng chịu lỗi cao. 

<!--more-->

## Giới thiệu

Ceph là dự án mã nguồn mở, cung cấp giải pháp lưu trữ dữ liệu. Ceph cung cấp giải pháp lưu trữ phân tán mạnh mẽ, đáp ứng các yêu cầu về khả năng mở rộng, hiệu năng, khả năng chịu lỗi cao. Xuất phát từ mục tiêu ban đầu, Ceph được thiết kết với khả năng mở rộng không giới hạn, hỗ trợ lưu trữ tới mức exabyte, cùng với khả năng tương thích cao với các phần cứng có sẵn. Và từ đó, Ceph trở nên nổi bật trong ngành công nghiệp lưu trữ đang phát triển và mở rộng. 
Hiện nay, các nền tảng hạ tầng đám mây công cộng, riêng và lai dần trở nên phổ biến và to lớn. Bên cạnh đó, phần cứng - thành phần quyết định hạ tầng đám mây đang dần gặp các vấn đề về hiệu năng, khả năng mở rộng. Ceph đã và đang giải quyết được các vấn đề doanh nghiệp đang gặp phải, cung cấp hệ thống, hạ tầng lưu trữ mạnh mẽ, độ tin cậy cao.

Nguyên tắc cơ bản của Ceph:
- Khả năng mở rộng tất cả thành phần;
- Khả năng chịu lỗi cao;
- Giải pháp dựa trên phần mềm, hoàn toàn mở, tính thích nghi cao;
- Chạy tương thích với mọi phần cứng;

Ceph xây dựng kiến trúc mạnh mẽ, khả năng mở rộng không giới hạn, hiệu năng cao, cung cấp giải pháp thống nhất, nền tảng mạnh mẽ cho doanh nghiệp, giảm bớt sự phụ thuộc vào phần cứng đắt tiền. Hệ sinh thái Ceph cung cấp giải pháp lưu trữ dựa trên block, file, object, và cho phép tùy chỉnh theo ý muốn. Nền tảng Ceph xây dựng dựa trên các object, tổ chức trên các block. Tất cả các kiểu dữ liệu, block, file đều được lưu dưới dạng object, quản trị bởi Ceph cluster. Object storage hiện đã trở thành giải pháp cho hệ thống lưu trữ truyền thống, cho phép xây dựng kiến trúc hạ tầng độc lập với phần cứng. 

Ceph xây dựng giải pháp dựa trên Object, Các object được tổ chức, nhân bản trên toàn cluster, nâng cao tính bảo đảm dữ liệu. Tại Ceph, Object sẽ không tồn tại đường dẫn vật lý, toàn bộ Object được quản trị dưới dạng Key Object, tạo nền tảng mở với khả năng lưu trữ tới hàng petabyte-exabyte.


![](/media/ceph-overview.png)


## Ceph và tương lai của hệ thống lưu trữ
### Ceph – Giải pháp cloud storage
Thành phần quan trọng để phát triển Cloud chính là hạ tầng lưu trữ hay còn gọi là Storage. Cloud cần storage để phát triển, đáp ứng nhu cấp lưu trữ, tổ chức lượng dữ liệu cực lớn. Hiện nay, các giải pháp lưu trữ truyền thống đã dần tới giới hạn như gặp phải các vấn đề về chí phí, kiến trúc, tính mở rộng v.v. Ceph đã giải quyết được các vấn để đang gặp phải, đáp ứng nhu cầu lưu trữ của Cloud.
Ceph hỗ trợ tốt các nền tảng Cloud nổi bật như OpenStack, CloudStack, OpenNebula. Đội ngũ phát triển và cộng tác của Ceph bao gồm các nhà cung cấp nền tảng lớn nhất (Canonical, Red Hat, SUSE), với nhiều năm kinh nghiệp cũng như nắm bắt được xu hướng phát triển, khiến Ceph luôn đi trước, bắt kịp thời đại, tương thích cao với Linux, thành một trong những hệ thống tốt nhất khi đánh giá xây dựng hạ tầng lưu trữ.

### Ceph – Giải pháp software-defined
Để tiết kiệm chi phí, Ceph xây dựng giải pháp dựa trên phần mềm Software-defined Storage (SDS). Cung cấp giải pháp đáp ứng các khách hàng đã có sẵn hạ tầng lớn, không mong muốn đầu tư thêm nhiều chi phí. SDS hỗ trợ tốt trên nhiều phấn cứng từ bất kỳ nhà cung cấp. Mang đến các lợi thế về giá thành, tính bảo đảm, và khả năng mở rộng.

### Ceph – Giải pháp lưu trữ thống nhất
Ceph mang đến giải pháp lưu trữ thống nhất bao gồm file-based và block-based access truy cập thống nhất qua nền tảng, giải pháp Ceph. Đáp ứng nhu cầu tăng trưởng dữ liệu hiện tại và cả trong tương lai. Ceph xây dựng giải pháp lưu trữ thống nhất (true unified storage solution) bao gồm object, block, file storage và đồng bộ qua nền tảng dựa trên phần mềm , hỗ trợ lưu trữ các luồng dữ liệu lớn, không có cấu trúc. Lợi dụng điểm mạnh của ceph, toàn bộ block hay file storage đều được lưu trữ dưới dạng đối tượng quản trị bởi Ceph Cluster. Ceph quản lý các object dưới kiến trúc riêng của mình. Object trong ceph được quản trị, tổ chức riêng biệt, và hỗ trợ mở rộng không giới hạn bằng cách lược bỏ các metadata, bỏ qua đường dẫn vật lý. Để làm được điều này, ceph sử dụng thuật toán động để tính toán, lưu trữ, tìm kiếm dữ liệu.

![](/media/ceph-solution.png)

## Kiến trúc thế hệ mới

Vấn đề của các hệ thống lưu trữ truyền thống là không có phương pháp quản lý metadata thông minh. Cơ bản, các metadata là thông tin về dữ liệu, quyết định dữ liệu sẽ được lưu trữ, truy vấn, đọc ghi tại đâu. Các hệ thống lưu trữ truyền thống cần duy trì trung tâm quản lý metadata, chúng có trách nhiệm tìm kiếm thông tin về metadata. Vì vậy, mỗi khi client yêu cầu hoạt động đọc ghi, storage sẽ tìm kiếm vị trí dữ liệu trong các bảng metadata rất lớn. Với hệ thống nhỏ, độ trễ sẽ không lớn nhưng đối với các hệ thống lớn thì độ trễ sẽ rất cao, hạn chế khả năng mở rộng.
Ceph không xây dựng giải pháp theo phương pháp truyền thống, nó sử dụng phát triển kiến trúc hoàn toàn mới, sử dụng phương pháp lưu trữ, tổ chức theo thuật toán động "CRUSH algorithm". CRUSH viết tắt "Controlled Replication Under Scalable Hashing". Thay vì tìm kiếm metadata theo bảng, thuật toán CRUSH sẽ dựa trên yêu cầu, tính toán vị trí dữ liệu, cải thiện tốc độ. Hơn thế, thuật toán được xử lý phân tán trên các cluster node, tận dụng sức mạnh tính toán phân tán (tính toán lưới). CRUSH quản lý các metadata tối ưu hơn rất nhiều khi so sánh với phương pháp truyền thống.

Bên cạnh đó, thuật toán CRUSH còn có khả năng nhận thức về hạ tầng. Hiểu được mối quan hệ giữa các thành phần trong hạ tầng như ổ đĩa hệ thống, các hồ chứa, các node, tủ rack, hệ thống điện, switch, các hàng máy chủ, các phòng máy chủ và còn hơn thế. Khi các thành phần xảy ra lỗi, CRUSH sẽ lưu trữ bản sao dữ liệu và nhân rộng chúng đến các phân vùng trong bộ nhớ, khiến dữ liệu luôn sẵn sàng, thống nhất. Đồng thời CRUSH cho phép cung cấp khả năng tự quản trị và tự sửa lỗi cho Ceph. Khi nhận thức lỗi, CRUSH sẽ tự sửa lỗi dữ liệu, tái phân bố dữ liệu trên toàn Cluster. Tại mọi thời điểm sẽ luôn có hơn một bản sao dữ liêu, nằm phân tán trong Cluster. Với CRUSH, Ceph tạo ra hạ tầng lưu trữ bảo đảm với độ tin cậy cao. Qua đó, Ceph đáp ứng các nhu cầu về mở rộng, đảm bảo hệ thống lưu trữ.

## Giới hạn của công nghệ Raid 

Công nghệ Raid được ứng dụng cho Storage rất nhiều năm trở lại đây, nó là công nghệ thành công nhất trong lĩnh vực khôi phục, chịu lỗi, tái tạo dữ liệu. Tuy nhiên, công nghệ RAID hiện tại đã tới giới hạn. Thời điểm hiện tại, nó đã bắt đầu xuất hiện những điểm yếu rõ rệt khi ứng dụng vào những công nghệ mới. Công nghệ sản xuất ổ cứng ngày càng hiện đại với giá thành giảm dần. Dung lượng dữ liệu đã lên tới 4TB-6TB, và tăng dần theo từng năm. Với khối lượng dữ liệu càng lớn, việc tái tạo dữ liệu bằng RAID sẽ tốn rất nhiều thời gian, có thể mất tới vài tiếng đền vài ngày để sửa lỗi. Và nếu hỏng nhiều thiết bị cùng lúc, việc sửa lỗi sẽ tốn đến ngày, tháng để khôi phục, tốn rất nhiều chi phí. Hơn hết Raid sử dụng nhiều tài nguyên, tăng TCO, và khi storage đến giới hạn, nó sẽ lại đẩy chi phí đầu tư lên. Đồng thời khi đầu tư RAID, ta phải quan tâm đến các thống số ở đĩa, sẽ rất phức tạp khi cần nâng cấp.
Việc sử dụng RAID cũng kéo theo các thiết bị phần cứng đắt tiền – RAID card. Chúng có thể phát sinh lỗi khi không thể lưu trữ thêm dữ liệu, bên cạnh đó là giới hạn phần cứng không thể thêm dung lượng kể cả khi đầu tư thêm chi phí. RAID tồn tại giới hạn về giải pháp: Raid 5 có thể chịu lỗi 1 ổ, Raid 6 2 ổ, nếu số ổ lỗi nhiều hơn, việc phục hồi dữ liệu sẽ trở nên rất khó khăn hoặc không thể khôi phục. Đồng thời, RAID chỉ có thể bảo vệ lỗi trên các ổ đĩa, còn các lỗi về mạng, phần cứng, hệ điều hành … chúng không thể giải quyết được.
Ceph là giải pháp cho những vấn đề trên. Về tính đảm bảo, Ceph nhân bản dựa trên thuật toán, không phụ thuộc vào RAID. Ceph xây dựng giải pháp dựa trên phần mềm (software-defined storage), vì vậy ta sẽ không cần sử dụng các thiết bị chuyên dụng. Đồng thời Ceph nhân bản dữ liệu dựa trên cấu hình, vì thế người quản trị sẽ dễ dàng định nghĩa số lượng bản sao, tối ưu phần cứng, dễ dàng quản lý dữ liệu. Ceph còn có khả năng chịu lỗi nhiều hơn 2 ổ cứng với tốc độ khôi phục đáng ngạc nhiên (nhanh hơn rất rất nhiều khi so sánh với RAID). Đồng thời Ceph có thể lưu trữ khối lượng dữ liệu rất lớn dựa vào thuật toán CRUSH, vào quản trị thông qua Cluster Map.
Bên cạnh phương pháp nhân bản, Ceph cung cấp "erasure-coding technique". Kỹ thuật này yêu cầu ít không gian lưu trữ hơn khi so sánh với replicated pool. Khi xử lý, dữ liệu sẽ được khôi phục và tái tạo bằng erasure-code calculation. Ta có thể sử dụng đồng thời 2 phương pháp trong giải pháp Ceph storage.

