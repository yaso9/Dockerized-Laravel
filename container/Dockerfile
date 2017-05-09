FROM php:7.1.4-apache

RUN apt-get update
RUN apt-get install -y git
RUN apt-get install -y unzip
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN curl -sL https://deb.nodesource.com/setup_6.x | bash -
RUN apt-get install -y nodejs
RUN npm install -g gulp
RUN npm install -g bower

ENTRYPOINT if [ -z $(find / | grep /laravel/) ]; \
then \

composer create-project laravel/laravel /laravel * \

&& sed -i "s/^DB_HOST=.*/DB_HOST=`getent hosts database | awk '{print $1}'`/" /laravel/.env \
&& sed -i "s/^DB_DATABASE=.*/DB_DATABASE=laravel/" /laravel/.env \
&& sed -i "s/^DB_USERNAME=.*/DB_USERNAME=root/" /laravel/.env \
&& sed -i "s/^DB_PASSWORD=.*/DB_PASSWORD=pwd123/" /laravel/.env; \

fi; \

sed -i "164s,^.*,<Directory /laravel>," /etc/apache2/apache2.conf \
&& sed -i "166s,^.*,	AllowOverride All," /etc/apache2/apache2.conf \
&& sed -i "12s,^.*, DocumentRoot /laravel/public," /etc/apache2/sites-available/000-default.conf \

&& service apache2 restart \
&& cd /laravel/public \
&& sleep infinity;
