---
title: "Các loại Load Balancer trong AWS"
description: "Application Load Balancer là giải pháp này hoạt động ở tầng ừng dụng (layer 7), phù hợp nhất để cân bằng lưu lượng HTTP và HTTPS. Gateway Load Balancer cho phép bạn triển khai, mở rộng, quản lý các ứng dụng 3th party network (Firewall...)"
author:
  fullname: Phan Văn Đức
  username: ducpv
  avatar: "/configs/author/ducpv.jpg"
category:
  name: "Khóa học AWS từ cơ bản đến nâng cao"
  slug: "aws"
chapter:
  name: "High Availability"
  slug: "chap-03-ha"
image: https://user-images.githubusercontent.com/29729545/163432577-faca8832-ac36-441c-8a62-2821101c2a29.png
position: 10
---

## Các loại Load Balancer

- Classic Load Balancer - 2009 - CLB
- Application Load Balancer - 2016 - ALB
- Network Load Balancer - 2017 - NLB
- Getway Load Balancer - 2020 - GLB Chúng ta nên sử dụng các phiên bản mới nhất để được hỗ trợ nhiều tính năng nhất có thể. Classic Load Balancer hiện tại không còn được sử dụng nhiều.

## Application Load Balancer trong AWS

![Application Load Balancer trong aws](https://d1.awsstatic.com/Digital%20Marketing/House/1up/products/elb/Product-Page-Diagram_Elastic-Load-Balancing_ALB_HIW%402x.cb3ce6cfd5dd549c99645ed51eef9e8be8a27aa3.png)

- Giải pháp này hoạt động ở tầng ứng dụng (layer 7), phù hợp nhất để cân bằng lưu lượng HTTP và HTTPS
- Load balancing đến nhiều HTTP ứng dụng qua nhiều machine
- Hỗ trợ redirect HTTP/HTTPs
- Hỗ trợ route đến nhiều target group
  - VD: sample/users và sample/pots sẽ gọi đến 2 target group khác nhau

![ALB route](https://user-images.githubusercontent.com/29729545/163432577-faca8832-ac36-441c-8a62-2821101c2a29.png)

- ALB rất phù hợp với ứng dụng micro service
- Chúng ta sẽ truy cập vào ALB trông qua 1 fixed hostname (xxx.region.elb.amazon.com)

## Network Load Balancer trong AWS

![Network Load Balancer trong aws](https://d1.awsstatic.com/Digital%20Marketing/House/1up/products/elb/Product-Page-Diagram_Elastic-Load-Balancing_NLB_HIW%402x.2f8ded8b565042980c4ad5f8ec57d6b2fafe54ba.png) Hoạt động ở layer 4 dùng để forward TCP&UDP traffic đến instance của bạn. có thể xử lý hàng triệu request trong 1 giây nhưng vẫn đảm bảo độ trễ thấp (~100ms, so với ALB ~400ms).

- NLB có 1 static IP mỗi AZ
- **Chú ý**: NLB không support free tier nên chú ý khi sử dụng.

## Gateway Load Balancer trong AWS

![Getway Load Balancer trong aws](https://d1.awsstatic.com/Digital%20Marketing/House/1up/products/elb/Product-Page-Diagram_Elastic-Load-Balancing_GWLB_HIW%402x.58547db68b537b4aa4b0cdf7e593a6415d588a09.png)

- GLB cho phép bạn triển khai, mở rộng, quản lý các ứng dụng 3th party network (Firewall...)
- Hoak động ở Layer 3 (Network layer)
- Sử dụng GENEVE protocol trên port 6081

## Sticky Session trong load balancer

![Sticky Session trong aws](https://user-images.githubusercontent.com/29729545/163432991-ae56d5dd-acf1-483a-a855-95cd36ad5657.png) Sticky Session là một tính năng trong một hệ thống cân bằng tải cho website. Khi 1 user gửi request lần đầu tiên đến được Load Banlancer chỉ định 1 server xử lý request đó, đến lần request sau, cũng sẽ vẫn là server đó xử lý request. Tính năng này chủ yếu được sử dụng để đảm bảo một in-proc session nào đó sẽ không bị mất bởi các yêu cầu cho session được route đến các máy chủ khác nhau.

- Option này chỉ hoạt động với Classic Load Banlancers & Application Load Balancers
- Để tính năng này hoạt động, Client phải hỗ trợ cookies

## Cross-Zone Load Balancing

![Cross-Zone Load Balancing](https://cloudacademy.com/wp-content/uploads/2019/08/Screen-Shot-2019-08-02-at-9.46.42-AM.png) Như hình vẽ chúng ta có thể hình dung được Cross-Zone Load Balancing là gì, ở bổi cảnh này chúng ta có 1 AZ là A và B, trong AZ A có 4 server và B có 4 server. AWS ELB cung cấp 2 option bât/tắt Cross-Zone Load Balancing (enable/disable)

- Khi enable: traffic sẽ được chia đều cho các target khác nhau, mỗi server có 10%
- Khi disable: một bên là 8.33% còn 1 bên nhận 12.5% traffic

::alert{type="infor"}
<strong> Application Load Balancer </strong>

  <ul>
    <li>Luôn bật (không thể tắt option này đi)</li>
    <li>Không bị tính phí inter AZ</li>
  </ul>
  <strong> Network Load Balancer </strong>
  <ul>
    <li>Mặc định sẽ disabled</li>
    <li>Nếu enable lên AWS sẽ tính phí inter AZ của bạn</li>
  </ul>
  <strong> Classic Load Balancer </strong>
  <ul>
    <li>Mặc định sẽ disabled</li>
    <li>Nếu enable, không bị tính phí inter AZ</li>
  </ul>
::
