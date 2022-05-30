## Cài postgresql & Ltree extension trên Ubuntu 18.04

### Step1: Cài các gói lân cận cần thiết

```
sudo apt update
sudo apt -y install curl vim bash-completion wget
sudo apt -y upgrade
```

### Step2: Add PostgreSQL 12 repository

- Cần nhập GPG key và thêm kho lưu trữ PostgreSQL 12 vào máy Ubuntu của chúng ta. Chạy các lệnh sau để thực hiện điều này.

```
curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc|sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg
```

- Sau khi nhập khóa GPG, hãy thêm nội dung kho lưu trữ vào hệ thống Ubuntu 22.04 / 20.04 / 18.04 / 16.04 của bạn:

```
echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" |sudo tee  /etc/apt/sources.list.d/pgdg.list
```

- Kho lưu trữ được thêm vào chứa nhiều gói khác nhau bao gồm các phần bổ trợ của bên thứ ba. Chúng bao gồm:

```
- postgresql-client

- postgresql

- libpq-dev

- postgresql-server-dev

- pgadmin packages
```

### Step3: Cài đặt PostgreSQL 12 trên Ubuntu 22.04 | 20.04 | 18.04 | 16.04

- Bây giờ kho lưu trữ đã được thêm thành công, hãy cập nhật danh sách gói và cài đặt các gói máy chủ và ứng dụng khách PostgreSQL 12 trên Ubuntu 22.04 / 20.04 / 18.04 / 16.04 của bạn.


```
sudo apt-get update

sudo apt -y install postgresql-12 postgresql-client-12


```

-  PostgreSQL được khởi động và thiết lập để xuất hiện sau mỗi lần khởi động lại hệ thống.

``` 
systemctl enable postgresql
systemctl status postgresql.service
```



### Kết quả các bước trên sau khi thành công sẽ như sau:

![image](https://user-images.githubusercontent.com/83824403/170861785-d5696542-2c49-4aff-af68-1100fef6e6c0.png)




## Enable **Ltree extenson**

### Step 1: cài các gói cần thiết

- **GNU readline** là một thư viện phần mềm cung cấp khả năng chỉnh sửa dòng và lịch sử cho các chương trình tương tác với giao diện dòng lệnh.

```
apt install  python3 wget git byobu telnet -y
apt-get install libreadline-dev
apt-get install zlib1g-dev
apt -y install build-essential
```
 
### Step2: Git clone thư mục code về và chuyển sang nhánh vltree-12 sau đó chạy script


```
git clone https://github.com/bizflycloud/postgres.git
cd postgres && git checkout vltree-12
./configure
```

Sau khi chạy scrip sẽ như sau:

![image](https://user-images.githubusercontent.com/83824403/170914056-b260f286-ea7b-4782-a427-4cd2db48b4b1.png)


### Step3: cài bison và cài đặt bison (GNU Parser Generator)


```
apt-get install flex bison
cd contrib/ltree
PATH=$PATH:/usr/local/m4/bin/
cd ..
cd ..
./configure --prefix=/root/postgres/contrib/ltree --with-libiconv-prefix=/usr/local/libiconv/
cd contrib/ltree
make
```
 - Sau khi chajy script sẽ như sau


![image](https://user-images.githubusercontent.com/83824403/170914428-5ceabe3f-37c9-4a03-a6e6-f3fe95ce5829.png)


- COPY `ltree.so`,... sang ``/postgres/lib`` và ``/postgres/extension`` của ubuntu

```
cp ltree.so /usr/lib/postgres/12/lib
cp ltree.control ltree--1.0--1.1.sql ltree--1.1.sql ltree--unpackaged--1.0.sql /usr/share/postgres/12/extension/
```


### Step 4: Test
- Truy cập postgre shell:

```
sudo -u postgres psql
```
- Set password cho user postgres:

```
postgres=# \password postgres
```
- Tạo Db test và Kiểm tra các database đang có :
```
postgres=# \l
```

![image](https://user-images.githubusercontent.com/83824403/170914963-a885bc9d-25a1-4996-a787-5c75c7211710.png)



- Khởi tạo các extension :
```
postgres=# create extension ltree;
postgres=# create extension "uuid-ossp";
```

- Kiểm tra lại các extension đã được cài đặt :
```
postgres=# \dx
```

![image](https://user-images.githubusercontent.com/83824403/170915079-52d88c1b-1462-4b33-a3a0-0cac333fcee2.png)


- Sửa bind IP trong file cấu hình ``/etc/postgres/12/main/pg_hba.conf`` thành :
```
# IPv4 local connections:
host    all             all             0.0.0.0/0               md5
```


- Sửa listen IP trong file cấu hình ``/etc/postgres/12/main/postgresql.conf`` thành :
```
# - Connection Settings -

listen_addresses = '*'

```

- Khởi động lại dịch vụ :
```

systemctl restart postgresql-12
```
- Test extension ltree. Tạo DB test -> enable extension ltree -> create table vs type ltree -> insert test.

```

postgres=# create extension ltree;
postgres=# CREATE TABLE item(id int,item_name ltree,PRIMARY KEY(id ));
postgres=# insert into item(id, item_name) values (1, 'home/opt/abc');
```
### Kết quả như sau 

![image](https://user-images.githubusercontent.com/83824403/170915342-cdbb4778-0e60-4ad9-ac61-f81f513503db.png)




# Reference + Fix bug

## Reference
- https://git.paas.vn/backup-service/endeavour/-/blob/staging/docs/install-step-by-step.md

## fix bug
- http://www.dark-hamster.com/operating-system/how-to-solve-source-compile-error-configure-error-no-acceptable-c-compiler-found-in-path-in-linux-ubuntu/?fbclid=IwAR3Vdp6fDOEpwdcBAHvQIJYEA6zpo87yG3T9FcwJ95R1-mB_IfpYwTsI_UY
- https://askubuntu.com/questions/89389/how-to-solve-configure-error-readline-library-not-found
- https://askubuntu.com/questions/1169754/configure-error-could-not-find-the-zlib-library
- https://askubuntu.com/questions/557629/how-to-install-flex-and-bison-error-can-not-locate-file
- https://geeksww.com/tutorials/miscellaneous/bison_gnu_parser_generator/installation/installing_bison_gnu_parser_generator_ubuntu_linux.php




