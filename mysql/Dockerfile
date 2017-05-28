FROM mysql:5.7

MAINTAINER copyrenzhe <copyrenzhe@gmail.com>

RUN chown -R mysql:root /var/lib/mysql/

COPY my.cnf /etc/mysql/conf.d/my.cnf

CMD ["mysqld"]

EXPOSE 3306