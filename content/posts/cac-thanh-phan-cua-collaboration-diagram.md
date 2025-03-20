---
title: "Các thành phần của Collaboration Diagram"
date: 2025-01-10
---

Sau khi hiểu rõ Collaboration Diagram là gì, bước tiếp theo là tìm hiểu các thành phần cơ bản cấu tạo nên loại sơ đồ này. Dưới đây là chi tiết từng thành phần của Collaboration Diagram, dựa trên tài liệu bạn cung cấp.

## 1\. Objects (Đối tượng)

**Định nghĩa:**  
  
Objects là các thực thể tham gia vào một kịch bản tương tác cụ thể trong hệ thống. Mỗi đối tượng đại diện cho một phiên bản của một lớp cụ thể trong hệ thống.

**Vai trò:**

- Xác định các đối tượng tham gia vào quá trình tương tác.
- Làm rõ cách các đối tượng được tổ chức trong hệ thống.

**Ví dụ:**  
  
Trong kịch bản "Rút tiền từ ATM", các đối tượng có thể bao gồm:

- **Customer**: Đại diện cho người dùng.
- **ATM**: Máy rút tiền.
- **BankAccount**: Tài khoản ngân hàng.

## 2\. Links (Liên kết)

**Định nghĩa:**  
  
Links là các kết nối giữa các đối tượng, biểu thị mối quan hệ giữa chúng. Links là các đường nối giữa các đối tượng trên sơ đồ.

**Vai trò:**

- Hiển thị cách các đối tượng có thể giao tiếp với nhau.
- Định nghĩa mối quan hệ giữa các đối tượng trong hệ thống.

**Ví dụ:**

- **Customer → ATM**: Khách hàng tương tác với máy ATM.
- **ATM → BankAccount**: Máy ATM truy vấn tài khoản ngân hàng.

## 3\. Messages (Thông điệp)

**Định nghĩa:**  
  
Messages biểu thị các tương tác giữa các đối tượng thông qua việc gửi và nhận thông điệp.

**Đặc điểm:**

- Mỗi thông điệp thường có một thứ tự để chỉ rõ trình tự thực hiện.
- Các thông điệp được thể hiện dưới dạng mũi tên chỉ hướng giao tiếp.

**Ví dụ:**

- **Customer gửi PIN đến ATM**: `1: inputPIN()`.
- **ATM yêu cầu xác minh từ BankAccount**: `2: verifyAccount()`.
- **BankAccount phản hồi đến ATM**: `3: respondToVerification()`.

## 4\. Sequence Numbering (Đánh số thứ tự)

**Định nghĩa:**  
  
Sequence Numbering được sử dụng để đánh dấu thứ tự thực thi của các thông điệp giữa các đối tượng.

**Vai trò:**

- Đảm bảo thứ tự thực hiện chính xác giữa các thông điệp.
- Làm rõ trình tự tương tác trong hệ thống.

**Ví dụ:**

1. `1: inputPIN()`
2. `2: verifyAccount()`
3. `3: dispenseCash()`

## 5\. Constraints (Ràng buộc)

**Định nghĩa:**  
  
Constraints là các quy tắc hoặc điều kiện được áp dụng trong quá trình tương tác giữa các đối tượng.

**Vai trò:**

- Giới hạn hành vi của hệ thống.
- Đảm bảo rằng các đối tượng tuân theo logic nghiệp vụ hoặc yêu cầu cụ thể.

**Ví dụ:**

- Chỉ rút tiền nếu số dư tài khoản >= số tiền yêu cầu rút.

**Tóm lại**, các thành phần trên giúp Collaboration Diagram thể hiện chi tiết cách các đối tượng giao tiếp và phối hợp để thực hiện chức năng cụ thể trong hệ thống. Đây là một công cụ quan trọng trong thiết kế và phân tích hệ thống hướng đối tượng.

Go to Source
