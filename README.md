"# openssl_ca" 
## 三、 证书生成

### 1. 根证书的生成

1. 新建目录root，并进入

2. 生成根密钥

   openssl genrsa -out rootca.key 2048

   如需加密可使用：openssl genrsa -aes256 -out rootca.key 2048

3. 创建自签名的根CA证书

   openssl req -sha256 -new -x509 -days 1826 -key rootca.key -out rootca.crt

   如有设置密码，需要输入rootca.key私钥保护口令。

   根据提示输入：国家、省份、组织名、邮箱、等信息。

   最后生成 rootca.crt 根证书文件。

4. 新建ca配置文件

   用于签发证书使用的一些配置，可以参考文章附件。

   

   

### 2. 使用根证书签发二级根证书

​	示例附件

1. 回到上层目录，新建目录secondary，并进入

2. 生成密钥

   openssl genrsa -out secondaryCA.key 2048

   如需加密可使用：openssl genrsa -aes256 -out secondaryCA.key 2048

3. 生成证书请求

   openssl req -new -sha256 -key secondaryCA.key -out secondaryCA.csr

   如有设置密码，需要输入secondaryCA.key私钥保护口令。

   根据提示输入：国家、省份、组织名、邮箱、等信息。

   最后生成secondaryCA.csr证书请求文件。

4. 签发证书

   openssl ca -batch -config ../root/ca.conf -notext -in secondaryCA.csr -out secondaryCA.crt

   最后生成证书 secondaryCA.crt

5. 新建二级ca配置文件

   用于签发证书使用的一些配置，可以参考文章附件。

   


### 3. 使用二级根证书签发用户证书

1. 回到上层目录，新建目录user，并进入

2. 生成密钥

   openssl genrsa -out user.key 2048

   如需加密可使用：openssl genrsa -aes256 -out user.key 2048

3. 生成证书请求

   openssl req -new -sha256 -key user.key -out user.csr

   如有设置密码，需要输入user.key私钥保护口令。

   根据提示输入：国家、省份、组织名、邮箱、等信息。

   最后生成user.csr证书请求文件。

4. 签发证书

   openssl ca -batch -config ../secondary/ca.conf -notext -in user.csr -out user.crt

   最后生成证书 user.crt

   
