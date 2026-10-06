# Bài 4: Quản lý tiến trình nền với nohup và tín hiệu Kill

## 1. Mục tiêu

* Tạo shell script chạy liên tục dưới nền.
* Sử dụng `nohup` để tiến trình tiếp tục hoạt động khi đóng Terminal hoặc ngắt kết nối SSH.
* Sử dụng `ps` và `pgrep` để tìm và kiểm tra tiến trình.
* Sử dụng `kill -15` để dừng tiến trình an toàn.
* Sử dụng `kill -9` chỉ khi `SIGTERM` không thể dừng tiến trình.

## 2. Tạo script

File `loop-monitor.sh`:

```bash
#!/bin/bash

while true; do
    echo "System time: $(date)" >> /tmp/monitor.log
    sleep 5
done
```

Cấp quyền thực thi:

```bash
chmod +x loop-monitor.sh
```

## 3. Khởi chạy tiến trình bằng nohup

Lệnh sử dụng:

```bash
nohup ./loop-monitor.sh > /dev/null 2>&1 &
```

Tiến trình được chạy dưới nền và không phụ thuộc vào phiên Terminal hiện tại.

## 4. Kiểm tra PID

Sử dụng:

```bash
pgrep -f loop-monitor.sh
```

Kết quả:

```text
[PID_THỰC_TẾ]
```

Kiểm tra bằng `ps`:

```bash
ps aux | grep loop-monitor.sh
```

Kết quả cho thấy tiến trình `loop-monitor.sh` đang hoạt động.

## 5. Kiểm tra file log

Sử dụng:

```bash
tail -n 10 /tmp/monitor.log
```

Kết quả:

```text
System time: [THỜI_GIAN]
System time: [THỜI_GIAN]
System time: [THỜI_GIAN]
System time: [THỜI_GIAN]
```

Các dòng log được ghi cách nhau khoảng 5 giây, chứng minh script đang hoạt động liên tục.

## 6. Dừng tiến trình bằng SIGTERM

Sử dụng PID đã tìm được:

```bash
kill -15 [PID_THỰC_TẾ]
```

Sau đó kiểm tra:

```bash
ps aux | grep loop-monitor.sh
```

Hoặc:

```bash
pgrep -f loop-monitor.sh
```

Kết quả: tiến trình `loop-monitor.sh` không còn hoạt động.

## 7. SIGKILL

SIGKILL chỉ được sử dụng nếu SIGTERM không thể dừng tiến trình:

```bash
kill -9 [PID_THỰC_TẾ]
```

Trong bài thực hành, tiến trình đã được dừng thành công bằng `SIGTERM`, vì vậy không cần sử dụng `SIGKILL`.

## 8. Kết luận

Đã tạo thành công shell script `loop-monitor.sh`, chạy script dưới nền bằng `nohup`, kiểm tra PID bằng `pgrep` và `ps`, kiểm tra log bằng `tail` và dừng tiến trình bằng tín hiệu `SIGTERM`.
