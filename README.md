
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
   #!/bin/sh
   xrdb $HOME/.Xresources
   startxfce4 &
   xfce4-terminal &
   ```

4. **Cấp quyền thực thi cho file `xstartup`**:

   ```bash
   chmod +x ~/.vnc/xstartup
   ```

5. **Khởi động lại VNC Server trên màn hình `:1` (tương ứng với cổng 5901)**:

   ```bash
   vncserver :1
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

Với các bước trên, bạn đã hoàn tất cài đặt và có thể điều khiển VPS qua VNC, sử dụng Google Chrome và truy cập vào Terminal XFCE tự động khi kết nối.
