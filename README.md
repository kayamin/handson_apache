# handson Apache x Springboot



`rpm -qi httpd`
- httpd パッケージ(Apache) の情報を表示

`rpm -ql httpd`
- httpd パッケージ(Apache) として取得されたファイルの一覧を表示


コンテナを立ち上げ，springboot の動作を確認

```
docker-compose up -d --build

❯ curl localhost:8090/hello
Hello World, this is SpringBoot Application.

❯ curl localhost:8090/addtwo/4
6
```


VirtualHost が２つ作成されていることを確認

- *:port で指定しておりかつ，どの VirtualHostにもマッチしない場合は最初に書かれた VirtualHost の内容が適用される
    - defaul server

```
❯ docker exec -it apache /bin/bash
[root@5196d6f9a4e4 /]# apachectl -t -D DUMP_VHOSTS
Passing arguments to httpd using apachectl is no longer supported.
You can only start/stop/restart httpd using this script.
If you want to pass extra arguments to httpd, edit the
/etc/sysconfig/httpd config file.
[Sat Jun 20 13:14:01.369669 2020] [so:warn] [pid 31] AH01574: module rewrite_module is already loaded, skipping
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 192.168.0.3. Set the 'ServerName' directive globally to suppress this message
VirtualHost configuration:
*:80                   is a NameVirtualHost
         default server test.com (/etc/httpd/conf/httpd.conf:361)
         port 80 namevhost test.com (/etc/httpd/conf/httpd.conf:361)
         port 80 namevhost spring.vhost.com (/etc/httpd/conf/httpd.conf:366)
```

VirtualHost に設定されているホスト名を指定しないと，springboot コンテナにプロキシされないことを確認

```
❯ curl localhost:80/hello
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>404 Not Found</title>
</head><body>
<h1>Not Found</h1>
<p>The requested URL /hello was not found on this server.</p>
</body></html>

❯ curl localhost:80/hello -H 'Host: spring.vhost.com'
Hello World, this is SpringBoot Application.
```