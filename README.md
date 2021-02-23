# API ck.confirm

Tài liệu hướng dẫn kết nối xác nhận giao dịch thanh toán từ CKBox.

***Lưu ý: CKBox sử dụng cơ chế đẩy giao dịch giống với VNPAY IPN [https://sandbox.vnpayment.vn/apis/docs/huong-dan-tich-hop/](https://sandbox.vnpayment.vn/apis/docs/huong-dan-tich-hop/), bạn có thể tham khảo tài liệu của VNPAY để hiểu rõ hơn cách kết nối với CKBox.**

### ENDPOINT GET
```
https://your-endpoint.com?checksum=559576976dc1bd21858626930f9bb339
      &amount=97000
      &bankDate=2021-02-22+03%3A52%3A04
      &receiver=84343861613
      &orderId=0
      &ckboxId=10001
      &txnRef=1000829
      &bankName=VIETCOMBANK
      &accountNumber=0451000285534
      &customerPhone=0363913022
      &balance=8859369
      &adminPhone=84946861101
      &sender=Vietcombank
      &orderInfo=0363913022.CT+tu+0351001150559+LA+VAN+BA+toi+0451000285534+CTY+C.
      &customerId=
      &smsBody=SD+TK+0451000285534+%2B97%2C000VND+luc+22-02-2021+03%3A52%3A04.+SD+8%2C859%2C369VND.+Ref+MBVCB82826102.0363913022.CT+tu+0351001150559+LA+VAN+BA+toi+0451000285534+CTY+C.
      &timestamp=1614069178000
```

### PAYLOAD POST
```
{
      "bankName": "VIETCOMBANK",
      "accountNumber": "0451000285534",
      "balance": "8859369",
      "amount": "97000",
      "bankDate": "2021-02-22 03:52:04",
      "orderInfo": "0363913022.CT tu 0351001150559 LA VAN BA toi 0451000285534 CTY C...",
      "customerPhone": "0363913022",
      "orderId": "0",
      "customerId": "",
      "ckboxId": "10001",
      "txnRef": "1000822",
      "adminPhone": "84946861101",
      "checksum": "d836f3f0100d403d628b06b8615577ae",
      "receiver": "84343861613",
      "sender": "Vietcombank",
      "timestamp": "1614066562000",
      "smsBody": "SD TK 0451000285534 +97,000VND luc 22-02-2021 03:52:04. SD 8,859,369VND. Ref MBVCB82826102.0363913022.CT tu 0351001150559 LA VAN BA toi 0451000285534 CTY C..."
}
```


Tham số | Kiểu dữ liệu |Bắt buộc/tùy chọn | Mô tả 
--------|--------------|------------------|-------
your-endpoint.com|Alphanumeric[10,100]|Bắt buộc|Website server khách hàng
bankName|Alpha[3,20]|Bắt buộc|Tên ngân hàng gửi BĐSD
accountNumber|Alphanumeric[10,20]|Bắt buộc|Số tài khoản ngân hàng BĐSD
balance|Numeric[1,20]|Bắt buộc|Số dư tài khoản
amount|Numeric[1,12]|Bắt buộc|Số tiền thanh toán không mang các ký tự phân tách thập phân, phần nghìn, ký tự tiền tệ.
bankDate|Numeric[14]|Bắt buộc|Thời gian giao dịch tại ngân hàng
orderInfo|Alphanumeric[1,70]|Bắt buộc|Nội dung chuyển khoản (Không dấu)
customerPhone|Numeric[10,11]|Bắt buộc|Số điện thoại trong nội dung chuyển khoản
orderId|Alphanumeric[3,25]|Bắt buộc|Mã đơn hàng trong nội dung chuyển khoản
customerId|Alphanumeric[1,25]|Bắt buộc|Mã khách hàng trong nội dung chuyển khoản
ckboxId|Numeric[5]|Bắt buộc|Mã CKBox, ví dụ: 11002
txnRef|Numeric[5,9]|Bắt buộc|Mã giao dịch do CKBox sinh ra (không trùng lặp)
checksum|Alphanumeric[32,256]|Tuỳ chọn|Mã kiểm tra (checksum), để đảm bảo dữ liệu của giao dịch không bị thay đổi trong quá trình chuyển từ CKBOX về Website khách hàng. Cần kiểm tra đúng checksum khi bắt đầu xử lý yêu cầu (trước khi thực hiện các yêu cầu khác).
password|Alphanumeric[6,25]|Tuỳ chọn|Mật khẩu do khách hàng tự đặt (nếu không dùng mã kiểm tra checksum).
receiver|Numeric[10,11]|Bắt buộc|Số điện thoại nhận BĐSD trên CKBox
sender|Numeric[1,15]|Bắt buộc|Tên ngân hàng (brandname) gửi tới CKBox
timestamp|Numeric[14]|Bắt buộc|Thời gian nhận tin nhắn trên CKBox
smsBody|Alphanumeric[1,160]|Bắt buộc|Nội dung đầy đủ của tin nhắn BĐSD

**Lưu ý**
- Website KH thực hiện kiểm tra sự toàn vẹn của dữ liệu (checksum) trước khi thực hiện các thao tác khác
- Thao tác cập nhật/xử lý kết quả sau khi thanh toán được thực hiện tại URL này
- Đây là URL server - call - server (CKBox gọi máy chủ website KH)
- Máy chủ KH trả dữ liệu lại cho CKBox bằng định dạng JSON

### BẢNG MÃ LỖI

Mã lỗi | Mô tả
------ | -----
00|Giao dịch thành công
01|CKBox không hợp lệ
02|Giao dịch đã tồn tại
03|Sai định dạng dữ liệu
04|Giao dịch không thành công
97|Chữ kí không hợp lệ
99|Các lỗi khác

### RESPONSE SUCCESS
```ruby
{
  "code": "00",
  "message": "Giao dịch thành công"
}
```
### RESPONSE ERROR EXAMPLE
```ruby
{
  "code": "97",
  "message": "Chữ kí không hợp lệ"
}
```
