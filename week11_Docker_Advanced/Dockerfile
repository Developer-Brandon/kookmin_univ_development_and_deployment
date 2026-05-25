FROM centos:7
MAINTAINER BrandonLee <brandon5959somi@gmail.com>
RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-Base.repo && \
    sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-Base.repo
RUN yum -y install epel-release
RUN yum -y install httpd
EXPOSE 80 443
CMD ["/usr/sbin/httpd", "-DFOREGROUND"]