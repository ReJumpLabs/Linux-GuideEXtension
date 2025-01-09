
# Hướng dẫn Cài đặt VNC, Môi trường Desktop XFCE và Google Chrome trên VPS

Dưới đây là hướng dẫn tổng hợp để **cài đặt VNC Server**, **thiết lập môi trường desktop** trên VPS Linux, **cài đặt Google Chrome**, và **mở Terminal XFCE tự động** khi kết nối qua VNC.

---

### 1. Cài đặt môi trường desktop XFCE trên VPS

Đầu tiên, cài đặt giao diện đồ họa XFCE:

```bash
sudo apt update
sudo apt install xfce4 xfce4-goodies -y
```

### 2. Cài đặt VNC Server

Cài đặt VNC Server để cho phép truy cập từ xa vào giao diện đồ họa của VPS:

```bash
sudo apt install tightvncserver -y
```

### 3. Thiết lập VNC Server

1. **Khởi động VNC Server lần đầu để tạo mật khẩu**:

   ```bash
   vncserver
   ```

2. **Dừng VNC Server** sau khi thiết lập mật khẩu:

   ```bash
   vncserver -kill :1
   ```

3. **Cấu hình VNC để sử dụng XFCE và mở Terminal tự động**:

   Mở file cấu hình `xstartup` và chỉnh sửa để VNC khởi động XFCE cùng với Terminal:

   ```bash
   nano ~/.vnc/xstartup
   ```

   Thay thế nội dung file bằng:

   ```bash
   #!/bin/bash
   xrdb $HOME/.Xresources
   startxfce4 &
   xfce4-terminal &
   -rfbport 6000
   -geometry 1920x1080 -depth 24

   ```

4. **Cấp quyền thực thi cho file `xstartup`**:

   ```bash
   chmod +x ~/.vnc/xstartup
   ```

### 4. Kết nối VNC từ máy Windows

1. Tải **VNC Viewer** từ [RealVNC](https://www.realvnc.com/en/connect/download/viewer/) và cài đặt trên máy Windows.
2. Mở VNC Viewer và kết nối tới `IP_VPS:1`, sau đó nhập mật khẩu VNC khi được yêu cầu.

   Bây giờ bạn sẽ thấy giao diện desktop của VPS và Terminal sẽ tự động mở khi bạn đăng nhập.

### 5. Cài đặt Google Chrome trên VPS

1. Mở Terminal (nếu chưa tự động mở trong phiên VNC).
2. **Tải Google Chrome**:

   ```bash
   wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
   ```

3. **Cài đặt Google Chrome**:

   ```bash
   sudo apt install ./google-chrome-stable_current_amd64.deb
   ```

   Nếu gặp lỗi phụ thuộc, chạy:

   ```bash
   sudo apt --fix-broken install
   ```

4. **Khởi động Chrome trong VNC** (thêm `--no-sandbox` nếu cần):

   ```bash
   google-chrome --no-sandbox
   ```

---


# Hướng Dẫn Cấu Hình VNC: Đổi Port và Cài Đặt Full HD

## 1. Đổi Port của VNC Server


## 2. Chạy VNC với màn hình Full HD

### **2.1. Đặt độ phân giải khi khởi động**
Để cấu hình màn hình với độ phân giải Full HD, sử dụng tùy chọn `-geometry` khi khởi động VNC:

```bash
vncserver :1 -rfbport 6000 -geometry 1920x1080 -depth 24
```

- `:1`: Display bạn muốn sử dụng.
- `-geometry 1920x1080`: Độ phân giải Full HD.
- `-depth 24`: Độ sâu màu 24-bit.

### **2.2. Cấu hình độ phân giải cố định**
Nếu muốn VNC luôn sử dụng độ phân giải Full HD:

#### **Với `xstartup`**
1. Mở file cấu hình `xstartup` (thường tại `~/.vnc/xstartup`).
2. Thêm lệnh sau:
   ```
   -geometry 1920x1080 -depth 24
   ```

---

## 3. Kiểm tra và xác minh

### **3.1. Xác minh port và độ phân giải**
- Xác minh port đang lắng nghe:
  ```bash
  netstat -tuln | grep <port>
  ```
- Kết nối qua VNC Viewer với địa chỉ:
  ```
  IP:<port>
  ```

### **3.2. Kiểm tra log VNC**
Nếu gặp vấn đề, kiểm tra log để tìm hiểu nguyên nhân:

```bash
cat ~/.vnc/<hostname>:<display>.log
```

---

> **Lưu ý:** Đảm bảo các cài đặt được áp dụng trong cấu hình tương ứng để không phải thực hiện lại mỗi lần khởi động.

---
