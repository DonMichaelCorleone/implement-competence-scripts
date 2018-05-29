FROM centos:7
MAINTAINER Loek Delahaye <ldelahaye@square.nl>
LABEL Vendor="CentOS" \
      License=GPLv2 \
      Version=2.4.6-40

# Never forget the -y flag when installing on CentOS since the docker image will crash because it cannot do confirmations
# Installs the Apache server
RUN yum -y --setopt=tsflags=nodocs update && \
    yum -y --setopt=tsflags=nodocs install httpd && \
    yum clean all && \
    echo -e "\x1b[01;96m Installing Apache finished! \x1b[0m" 


# # Set the working directory 
# WORKDIR /var/www/reader-docker.squarembs.com/www/

# # Copy all project and build it
# # This layer is rebuilt when ever a file has changed in the project directory
COPY . /var/www/reader-docker.squarembs.com/

# EXPOSE port 80 for web traffic to the apache server, this will be bridged on the host
EXPOSE 80

# EXPOSE port 443 for tls, this will be bridged on the host
EXPOSE 443

# Install php 5.6
RUN yum -y install epel-release && \
    yum -y install wget && \
    wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm && \
    wget https://centos7.iuscommunity.org/ius-release.rpm && \
    rpm -Uvh ius-release*.rpm && \
    yum -y update && \
    yum -y install php56u php56u-opcache php56u-xml php56u-mcrypt php56u-gd php56u-devel php56u-mysql php56u-intl php56u-mbstring php56u-bcmath php56u-soapRRUN && \
    php -v && \
    echo -e "\x1B[01;96m Installing php 5.6 finished! \x1B[0m" 

# Gcc compiler needed for ghostscript
RUN yum -y groupinstall "Development tools" && \
    echo -e "\x1b[01;96m Installing development tools finished! \x1b[0m" 

# Install pdftk
RUN yum -y localinstall https://www.linuxglobal.com/static/blog/pdftk-2.02-1.el7.x86_64.rpm &&\
    pdftk --version

# Install pdfinfo
RUN yum -y install poppler-utils && \
    pdfinfo -v

# Install vim and net-tools for debugging when in container
RUN yum -y install vim && \
    echo -e "\x1b[01;96m Installing vim finished! \x1b[0m" && \
    yum -y install net-tools && \
    echo -e "\x1b[01;96m Installing net-tools finished! \x1b[0m" 

ADD run-httpd.sh /run-httpd.sh
RUN chmod -v +x /run-httpd.sh

CMD ["/run-httpd.sh"]