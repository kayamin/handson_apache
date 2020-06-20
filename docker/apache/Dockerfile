FROM centos:centos7

RUN yum update -y && yum install -y httpd tree net-tools

EXPOSE 80

# Docker image のビルドの際にキャッシュが効くように，頻繁に変更する設定ファイルは後に持ってくる
COPY conf/* /etc/httpd/conf/

CMD ["/usr/sbin/httpd","-DFOREGROUND"]