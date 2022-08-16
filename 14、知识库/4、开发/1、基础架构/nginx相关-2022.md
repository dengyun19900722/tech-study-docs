

# Nginx配置Https

## 生成证书文件

```shell
#Step1：生成密钥
[root@rzy ~]# openssl genrsa -des3 -out server.key 2048    #生成密钥
Generating RSA private key, 1024 bit long modulus
................++++++
............................++++++
e is 65537 (0x10001)
Enter pass phrase for server.key: #输入密码
Verifying - Enter pass phrase for server.key:  #确认密码

#Step2：生成证书认证文件
[root@rzy ~]# openssl req -new -key server.key -out server.csr #生成证书认证文件
Enter pass phrase for server.key:  #之前的密码
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:CN  #国家
State or Province Name (full name) []:beijing  #省
Locality Name (eg, city) [Default City]:beijing  #市
Organization Name (eg, company) [Default Company Ltd]:baidu #公司
Organizational Unit Name (eg, section) []:  #邮箱，可以不输入
Common Name (eg, your name or your server's hostname) []:www.aaa.com  #域名，这个域名必须和nginx使用的域名相同
Email Address []:  #不用写

Please enter the following 'extra' attributes 
to be sent with your certificate request
A challenge password []:  #不用写
An optional company name []:  #不用写
[root@rzy ~]# ll
总用量 1028
-rw-------. 1 root root    1264 1月  12 18:27 anaconda-ks.cfg
-rw-r--r--  1 root root 1039530 4月  19 10:03 nginx-1.18.0.tar.gz
-rw-r--r--  1 root root     627 4月  26 23:21 server.csr
-rw-r--r--  1 root root     951 4月  26 23:21 server.key

#Step3：复制一份密码
[root@rzy ~]# cp server.key server.key.org  #复制一份密码

#Step4：删除密码，更安全
[root@rzy ~]# openssl rsa -in server.key.org -out server.key  #删除密码，更安全
Enter pass phrase for server.key.org:  #之前的密码
writing RSA key

#Step:5：生成公钥也就是证书
[root@rzy ~]# openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt #生成公钥也就是证书
Signature ok
subject=/C=CN/ST=beijing/L=beijing/O=baidu/CN=www.aaa.com
Getting Private key

```



## 修改nginx配置

```shell
worker_processes  1;
events {
     worker_connections  1024;
}
http {
     server {
        listen  80;
        #server_name  www.aaa.com;
        rewrite ^(.*)$  https://$host$1 permanent;
     }
     server {
         listen       443 default ssl;
         #ssl On;
         ssl_certificate /usr/local/nginx/cert/server.crt;  #指定证书路径
         ssl_certificate_key /usr/local/nginx/cert/server.key; #指定私钥路径
         #server_name  www.aaa.com; #指定域名，要和证书认证文件的域名相同
         location / {
             root   /usr/share/nginx/html;
             index  index.html index.htm;
         }
         error_page   500 502 503 504  /50x.html;
         location = /50x.html {
             root   html;
         }
     }
}
```



## 运行nginx容器

```shell
docker run  --name nginx -p 80:80 -p 443:443 -v /home/test/nginx.conf:/etc/nginx/nginx.conf -v /home/test/server.crt:/usr/local/nginx/cert/server.crt -v /home/test/server.key:/usr/local/nginx/cert/server.key nginx
```

