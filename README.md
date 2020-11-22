


=> Example: https://magento2.test

# Enter `php` container
docker-compose exec php bash

cd /var/www/html/magento

# Setup file permissions, except folder `dev/docker`
find var generated vendor pub/static pub/media app/etc -type f -exec chmod g+w {} + \
    && find var generated vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} + \
    && chown -R :www-data $(ls -Idev/docker)

# Run this to install db if it is new installation,
# or you can import your SQL in adminer: http://localhost:8088
php bin/magento setup:install \
    --db-host=mysql \
    --db-name=magento_db \
    --db-user=magento \
    --db-password=secret \
    --backend-frontname=admin_mn \
    --admin-firstname=Super \
    --admin-lastname=Admin \
    --admin-email=admin@example.com \
    --admin-user=admin \
    --admin-password=s3cret@123

# Update config
# Config to use varnish
php bin/magento config:set system/full_page_cache/caching_application 2
php bin/magento config:set system/full_page_cache/varnish/backend_host nginx
php bin/magento config:set system/full_page_cache/varnish/backend_port 80
# Base url and https
php bin/magento config:set web/unsecure/base_url http://magento2.test/
php bin/magento config:set web/secure/base_url https://magento2.test/
php bin/magento config:set web/secure/use_in_frontend 1
php bin/magento config:set web/secure/use_in_adminhtml 1
php bin/magento config:set web/seo/use_rewrites 1
# Locale, timezone, currency
php bin/magento config:set general/locale/code en_US
php bin/magento config:set general/locale/timezone Asia/Ho_Chi_Minh
php bin/magento config:set currency/options/base VND
# Flush cache
php bin/magento config:set cache:flush
```
