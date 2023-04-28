FROM ubuntu:22.04

ENV DEBIAN_FRONTEND noninteractive

RUN apt update
RUN apt install -y curl apache2 apache2-utils php libapache2-mod-php mysql-server
RUN apt install -y php-pspell php-curl php-gd php-intl php-mysql php-xml php-xmlrpc php-ldap php-zip php-soap php-mbstring

RUN rm /var/www/html/index.html

RUN echo "[mysqld]" >> /etc/mysql/my.cnf; echo "bind-address = 0.0.0.0" >> /etc/mysql/my.cnf

RUN service mysql start; mysql -e "CREATE USER 'apteka'@'localhost' IDENTIFIED BY 'apteka';"; mysql -e "GRANT ALL PRIVILEGES ON *.* TO 'apteka'@'localhost' WITH GRANT OPTION;"
RUN service mysql start; mysql -e "CREATE USER 'apteka'@'%' IDENTIFIED BY 'apteka';"; mysql -e "GRANT ALL PRIVILEGES ON *.* TO 'apteka'@'%' WITH GRANT OPTION;"
RUN service mysql start; mysql -e "FLUSH PRIVILEGES;"
RUN service mysql start; mysql -e "ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'root';"

COPY apteka/ /var/www/html/
COPY apteka_db.sql /home

RUN cd /home; service mysql start; mysql -u apteka -papteka -e "CREATE DATABASE apteka_db"; mysql -u apteka -papteka apteka_db < apteka_db.sql

ARG time=6000
ENV envtime=$time

EXPOSE 80
EXPOSE 3306

CMD ["sh", "-c", "service mysql start; service apache2 start; sleep ${envtime}"]
