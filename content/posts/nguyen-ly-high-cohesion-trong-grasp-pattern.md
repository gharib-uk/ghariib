---
title: "Nguyên lý High Cohesion trong GRASP Pattern"
date: 2025-01-10
---

## 1\. Nguyên lý High Cohesion là gì?

**High Cohesion** (Tính liên kết cao) là một nguyên tắc quan trọng trong thiết kế phần mềm hướng đối tượng. Nguyên tắc này đảm bảo rằng mỗi lớp hoặc module chỉ chịu trách nhiệm cho một nhóm nhỏ các chức năng liên quan chặt chẽ với nhau. Mục tiêu của nguyên lý này là tạo ra các lớp dễ duy trì, dễ hiểu và có thể tái sử dụng cao.

Một dấu hiệu của thiết kế tốt là mỗi lớp chỉ có một số lượng nhỏ các phương thức, và mỗi lớp đại diện cho một "thực thể" cụ thể từ thế giới thực. Điều này giúp giảm thiểu sự phức tạp trong hệ thống và làm rõ vai trò của từng lớp.

**Ví dụ**:  
Trong hệ thống quản lý thang máy, một lớp có chức năng kiểm soát nhiều hoạt động như báo động, mở/đóng cửa, ghi nhận lỗi và di chuyển thang máy sẽ được coi là **thiết kế tồi** vì không tập trung vào một vai trò cụ thể. Thay vào đó, các chức năng này nên được tách thành các lớp riêng biệt như:

- Lớp quản lý báo động (`Alarm`).
- Lớp điều khiển cửa thang máy (`Door Controller`).
- Lớp ghi nhật ký lỗi (`Fault Log`).

## 2\. Lợi ích của High Cohesion

- **Dễ bảo trì**: Khi mỗi lớp chỉ tập trung vào một nhiệm vụ, việc sửa lỗi hoặc thêm chức năng mới trở nên dễ dàng hơn.
- **Dễ hiểu**: Các lớp có chức năng rõ ràng, giúp lập trình viên nhanh chóng nắm bắt được vai trò của từng lớp.
- **Tái sử dụng cao**: Các lớp độc lập và được thiết kế để thực hiện một nhiệm vụ cụ thể có thể được sử dụng lại trong các hệ thống khác.
- **Tăng tính linh hoạt**: Một lớp có tính liên kết cao sẽ dễ dàng điều chỉnh hoặc mở rộng mà không ảnh hưởng đến các lớp khác.
- **Hỗ trợ nguyên lý SOLID**: High Cohesion thúc đẩy việc áp dụng các nguyên lý thiết kế như Single Responsibility Principle (SRP), làm cho phần mềm dễ mở rộng và bảo trì.

## 3\. Một số vấn đề cần xem xét thêm

- **Giữ vai trò cụ thể cho từng lớp**: Một lớp nên đại diện cho một khái niệm hoặc thực thể từ thế giới thực.
- **Không kết hợp quá nhiều trách nhiệm vào một lớp**: Điều này giúp tránh việc lớp trở nên quá phức tạp và khó hiểu.
- **Kết hợp với Low Coupling**: Đảm bảo rằng các lớp có sự liên kết cao nhưng ít phụ thuộc lẫn nhau.

## 4\. Kết luận

Việc áp dụng nguyên lý High Cohesion giúp xây dựng hệ thống phần mềm dễ bảo trì, dễ mở rộng và có tính tái sử dụng cao. Kết hợp với các nguyên tắc khác như Low Coupling sẽ tăng cường khả năng thiết kế và đảm bảo chất lượng của phần mềm.

Go to Source
