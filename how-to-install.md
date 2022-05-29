## Cài postgresql+ltree extension trên ubuntu 18

## Step1: Cài các gói lân cận cần thiết

```
sudo apt update
sudo apt -y install vim bash-completion wget
sudo apt -y upgrade
```

### Step2: Add PostgreSQL 12 repository

- Cần nhập khóa GPG và thêm kho lưu trữ PostgreSQL 12 vào máy Ubuntu của chúng ta. Chạy các lệnh sau để thực hiện điều này.

```
curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc|sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg
```

- Sau khi nhập khóa GPG, hãy thêm nội dung kho lưu trữ vào hệ thống Ubuntu 22.04 / 20.04 / 18.04 / 16.04 của bạn:

```
echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" |sudo tee  /etc/apt/sources.list.d/pgdg.list
```

- Kho lưu trữ được thêm vào chứa nhiều gói khác nhau bao gồm các phần bổ trợ của bên thứ ba. Chúng bao gồm:

```
postgresql-client

postgresql

libpq-dev

postgresql-server-dev

pgadmin packages
```

## Step3: Cài đặt PostgreSQL 12 trên Ubuntu 22.04 | 20.04 | 18.04 | 16.04

- Bây giờ kho lưu trữ đã được thêm thành công, hãy cập nhật danh sách gói và cài đặt các gói máy chủ và ứng dụng khách PostgreSQL 12 trên hệ thống Linux Ubuntu 22.04 / 20.04 / 18.04 / 16.04 của bạn.
```
 sudo apt-get update

sudo apt -y install postgresql-12 postgresql-client-12


```

-  PostgreSQL được khởi động và thiết lập để xuất hiện sau mỗi lần khởi động lại hệ thống.

``` 
systemctl enable postgresql
 systemctl status postgresql.service
```

### Kết quả các bước trên sau khi thành công trông như sau:

![image](https://user-images.githubusercontent.com/83824403/170861785-d5696542-2c49-4aff-af68-1100fef6e6c0.png)

## Enable Ltree extenson

## Step 1: cài các gói cần thiết

```
sudo apt install  python3 wget git byobu telnet -y
```

http://www.dark-hamster.com/operating-system/how-to-solve-source-compile-error-configure-error-no-acceptable-c-compiler-found-in-path-in-linux-ubuntu/?fbclid=IwAR3Vdp6fDOEpwdcBAHvQIJYEA6zpo87yG3T9FcwJ95R1-mB_IfpYwTsI_UY
https://askubuntu.com/questions/89389/how-to-solve-configure-error-readline-library-not-found
https://askubuntu.com/questions/1169754/configure-error-could-not-find-the-zlib-library
https://geeksww.com/tutorials/miscellaneous/bison_gnu_parser_generator/installation/installing_bison_gnu_parser_generator_ubuntu_linux.php




