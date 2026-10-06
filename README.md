# Bài 1: Khảo sát FHS và Phân quyền File/Folder nâng cao

## 1. Mục tiêu

* Tạo cấu trúc thư mục theo hệ thống FHS của Linux.
* Thực hành phân quyền thư mục bằng lệnh `chmod`.
* Thực hành thay đổi chủ sở hữu và nhóm sở hữu bằng lệnh `chown`.
* Kiểm tra cấu trúc thư mục và quyền truy cập bằng lệnh `ls -la`.

## 2. Môi trường thực hành

* Hệ điều hành: Ubuntu trên WSL.
* Tài khoản thực hiện: Tài khoản Linux thông thường.
* Thư mục ứng dụng: `/var/www/my-app`
* Chủ sở hữu: Tài khoản Linux hiện tại.
* Nhóm sở hữu: `www-data`.

## 3. Các lệnh đã thực hiện

### Bước 1: Kiểm tra tài khoản

```bash
whoami
id
```

### Bước 2: Tạo cấu trúc thư mục

```bash
sudo mkdir -p /var/www/my-app/public
sudo mkdir -p /var/www/my-app/logs
```

Kiểm tra cấu trúc thư mục:

```bash
ls -la /var/www/my-app
```

### Bước 3: Phân quyền thư mục public

```bash
sudo chmod 750 /var/www/my-app/public
```

Quyền `750` gồm:

* Owner: `rwx` — đọc, ghi và truy cập.
* Group: `r-x` — đọc và truy cập.
* Others: `---` — không có quyền.

Kiểm tra:

```bash
ls -ld /var/www/my-app/public
```

### Bước 4: Phân quyền thư mục logs

```bash
sudo chmod 770 /var/www/my-app/logs
```

Quyền `770` gồm:

* Owner: `rwx` — toàn quyền.
* Group: `rwx` — toàn quyền.
* Others: `---` — không có quyền.

Kiểm tra:

```bash
ls -ld /var/www/my-app/logs
```

### Bước 5: Thay đổi chủ sở hữu và nhóm sở hữu

Kiểm tra nhóm `www-data`:

```bash
getent group www-data
```

Gán tài khoản hiện tại làm chủ sở hữu và `www-data` làm nhóm sở hữu:

```bash
sudo chown -R "$USER":www-data /var/www/my-app
```

Kiểm tra quyền sở hữu:

```bash
ls -ld /var/www/my-app/public /var/www/my-app/logs
```

### Bước 6: Kiểm tra kết quả cuối cùng

Lệnh kiểm tra theo yêu cầu đề bài:

```bash
ls -la /var/www/my-app
```

**Kết quả thực tế:** Dán nguyên văn đầu ra của lệnh `ls -la /var/www/my-app` tại đây sau khi hoàn thành thực hành.

## 4. Kết quả mong đợi

| Thư mục  | Quyền số | Quyền ký tự  | Nhóm sở hữu |
| -------- | -------- | ------------ | ----------- |
| `public` | `750`    | `drwxr-x---` | `www-data`  |
| `logs`   | `770`    | `drwxrwx---` | `www-data`  |

Chủ sở hữu của cả hai thư mục là tài khoản Linux thông thường đang sử dụng.

## 5. Kết luận

Đã thực hành tạo cấu trúc thư mục `/var/www/my-app/public` và `/var/www/my-app/logs`, thiết lập quyền truy cập bằng `chmod`, thay đổi chủ sở hữu và nhóm sở hữu bằng `chown`, đồng thời kiểm tra kết quả bằng `ls -la`.

Thư mục `public` cho phép chủ sở hữu đọc, ghi và truy cập; nhóm được đọc và truy cập; người dùng khác không có quyền. Thư mục `logs` chỉ cấp toàn quyền cho chủ sở hữu và nhóm sở hữu, không cấp quyền cho người dùng khác.
