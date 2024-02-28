# Key:
- wyomind
``` cmd
composer config repositories.wyomind composer https://support_sci_sport_co_uk:7db17e3cb0a1e5dbdb18e571@repo.wyomind.com
```
- amasty:
	- user 
	```
	d411240c7e5cd7f770aab5461a231358
	```
	- pass 
	```
	f91314643691771610d03edb97a3c1ca
	```
- plumrocket
	- user 
	```
	TNq06I9VNR2IyuRaLQd5VRUujKjKU20q
	```
	- pass 
	```
	J5BIXJg7sCJAiJIK5EwtxYyxrh8L8Ka4
	```

# Cài đặt magento:
## Cách 1: Cài đặt bằng composer
### Step 1:
```
composer install
```
### Step 2:
```cmd
php bin/magento setup:install \
--base-url=http://{url.loc} \
--db-host=mysql \
--db-name={database-name} \
--db-user={user-name} \
--db-password= \
--admin-firstname=admin \
--admin-lastname=admin \
--admin-email=admin@admin.com \
--admin-user=admin \
--admin-password=admin123 \
--language=en_US \
--currency=USD \
--timezone=America/Chicago \
--use-rewrites=1 \
--search-engine=elasticsearch7 \
--elasticsearch-host=elastic \
--elasticsearch-port=9200 \
--elasticsearch-index-prefix=magento2 \
--elasticsearch-timeout=15

```
**=> Hoàn tất**

## Cách 2: khi đã đầy đủ
### Step 1: 
```cmd
php bin/magento s:d:c
```

# Cài đặt database với dung lượng lớn:
```cmd
gunzip -c {url.sql.zip} | mysql -h localhost -P 3306 --protocol=tcp -u {user-name} -p {database-name}

mysql -h localhost -P 3306 --protocol=tcp -u {user-name} -p {db-name} < {url.sql}
```
------------------------------------------------------------------------------------------

# Cài đặt devilbox:
## Docker, docker composer

Mở terminal và run lần lượt các command line sau:
```cmd
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg lsb-release -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
=> Hoàn thành việc cài đặt docker và docker compose. Kiểm tra: `docker-compose --version`

## Devilbox
Dowload file và giải nén trong thư mục `var/www/`` (nếu chưa có thư mục thì tạo thư mục `www` - `sudo mkdir /var/www/``)

https://drive.google.com/file/d/1r3SzmKvK3KANlgOBvjb3T2OcN0eE6qLw/view?usp=share_link

Coppy file `docker-compose.override.yml` trong thư mục `compose` ra thư mục devilbox

run lệnh: `sudo chmod 777 -R devilbox/`

để cấp quyền cho thư mục. Sau đó truy cập thư mục devilbox: `cd devilbox/`

sửa file run.sh:
	*docker-compose up -d php httpd mysql elasticsearch;* --> *docker-compose up -d php httpd mysql elastic;*	

Run: `sudo sh run.sh`

=> Nếu thành công thì terminal sẽ như sau:
![img.png](img.png)

Lưu ý là các câu lệnh từ giờ sẽ chỉ được run trong phạm vi terminal này, `/shared/httpd` <=> `/var/www/devilbox/data/www/``

## Cách tạo một project mới

Tạo một thư mục mới trong `/var/www/devilbox/data/www/` (tên thư mục sẽ tương đương với tên miền của project)

Ví dụ: folder name là `test` => tên miền sẽ là:test.loc

##### Cấu trúc một thư mục project mới:
|—> tên project
|———> htdocs (tên mặc định)
|———> pub (tên mặc định) sẽ chứa các mã php, css, html ….

Sau bước này cần đăng ký tên miền trong file host ở commad line nhập như sau:
```cmd
sudo subl /etc/hosts
```

sau đó thêm dòng mới theo như sau:
```
127.0.0.1	<project-name>.loc
```
![img_1.png](img_1.png)
Sau đó có thể truy cập http://test.loc để kiểm tra

Một số tính năng cần chú ý trong devilbox:
- http://localhost/vhosts.php : vị trí danh sách các vitual host (project) đã được khởi tạo thành công
- http://localhost/index.php : thông tin chung của devilbox
- https://i.imgur.com/HnpkC1d.png : vị trí để truy cập phpmyadmin
Các hostname:
- Mysql: mysql
- Elasticsearch: elastic

## Cài đặt Magento 2
https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/overview.html

- B1, tạo new devilbox project (Ex: magento2) , tạo database  (magento2)
- B2, Đăng ký tài khoản Adobe: https://marketplace.magento.com/
- B3, truy cập my Adobe account: https://marketplace.magento.com/customer/account/

để tạo các adobe key:
![img_2.png](img_2.png)

=> Create A New Access Key:
![img_3.png](img_3.png)

- B4: truy cập devilbox terminal ( https://i.imgur.com/yNIk1q0.png ) và truy cập đến project mới được tạo ( Ex: cd magento2/ )

run termial sau:
```cmd
composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition=<version> <install-directory-name>
```
Ex:
```cmd
composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition=2.4.3 htdocs/
```

Lưu ý nếu gặp vân đề sau:
![img_4.png](img_4.png)

Thì cần thực hiện run thêm các command line sau:
```cmd
composer.phar global config http-basic.repo.magento.com <public_key> <private_key>
```

Sau đó tiếp tục run lại lệnh create ở trên

Sau khi thành công tải Magento => truy cập vào thư mục htdocs để tiếp tục các bước cài đặt site: `cd htdocs/``

Cấp quyền cho các thư mục cần thiết:
```cmd
find var generated vendor pub/static pub/media app/etc -type f -exec chmod g+w {} +
find var generated vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} +
chown -R :www-data . # Ubuntu
chmod u+x bin/magento


php bin/magento setup:install \
--base-url= {http://url.loc} \
--db-host=mysql \
--db-name= {database-name} \
--db-user=root \
--db-password= \
--admin-firstname=admin \
--admin-lastname=admin \
--admin-email=admin@admin.com \
--admin-user=admin \
--admin-password=admin123 \
--language=en_US \
--currency=USD \
--timezone=America/Chicago \
--use-rewrites=1 \
--search-engine=elasticsearch7 \
--elasticsearch-host=elastic \
--elasticsearch-port=9200 \
--elasticsearch-index-prefix=magento2 \
--elasticsearch-timeout=15
```

tham khảo về magento2 command line:
```link
https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/config-cli.html
```

Sau đó cần khai báo vitual host trong file hosts là có thể kiểm tra:
- frontend site: http://magento2.loc
- backend site: http://magento2.loc/admin <có thể tìm backend frontName ở file htdocs/app/etc/env.php - https://i.imgur.com/bKRB1uy.png  >

*Lưu ý:* để connect vào backend thì cần thiết lập hoặc tắt module `Magento_TwoFactorAuth` ( ở giai đoạn này thì nên tắt )

## Cách hiển thị lỗi trong Magento 2:

1. htdocs/app/bootstrap.php  - https://i.imgur.com/r9htTVD.png
![img_5.png](img_5.png)

2. Đổi tên trong thư mục htdocs/pub/errors:
`local.xml.sample ` -> `local.xml`
3. Đổi Magento mode sang developer
```cmd
bin/magento deploy:mode:set developer
```

### CLI Magento Basic:
```cmd
php -d memory_limit=-1 bin/magento {simple}:deploy

php -d memory_limit=-1 bin/magento setup:upgrade

# Khởi tạo các static view file trong thư mục pub/:
php -d memory_limit=-1 bin/magento setup:static-content:deploy -f

# Khởi tạo các file cần biên dịch:
php -d memory_limit=-1 bin/magento setup:di:compile

# Flush Magento cache: 
php -d memory_limit=-1 bin/magento cache:flush

# Flush Cache Storage: 
php -d memory_limit=-1 bin/magento cache:clean

```

## File start.sh:
```cmd
#!/bin/sh

docker compose stop;
docker compose rm -f;
docker compose up -d php httpd mysql elastic;
```





