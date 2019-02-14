# LAMP

## check version
```shell
# apache
rpm -q httpd
# or
/usr/sbin/httpd -v
# or
/usr/sbin/apache2 -v

# php
php -v
# or 
echo "<?php phpinfo();?>" > /path/to/website/phpinfo.php
# then visit http:://website/phpinfo.php to get php version
```

* [How can I tell what version of apache I'm running?](https://unix.stackexchange.com/questions/6792/how-can-i-tell-what-version-of-apache-im-running)
* [How to check the PHP version on Linux](https://www.thegeekdiary.com/how-to-check-the-php-version-on-linux/)
