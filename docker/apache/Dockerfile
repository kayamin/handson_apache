FROM centos:centos7

RUN yum update -y && yum install -y httpd tree net-tools

EXPOSE 80

CMD ["/usr/sbin/httpd","-DFOREGROUND"]