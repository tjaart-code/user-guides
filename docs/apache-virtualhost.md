# Setting Up an Apache Virtual Host for Joomla

This tutorial configures Apache to serve your Joomla site at `/var/www/html/mySite` using a custom local domain (e.g. `mySite.local`) instead of `http://127.0.0.1/mySite`.

## Prerequisites

- Apache installed (part of your LAMP stack)
- Joomla site already at `/var/www/html/mySite`
- Root/sudo access

## 1. Create the Virtual Host Config File

```bash
sudo subl /etc/apache2/sites-available/mySite.conf
```

Paste the following:

```apacheconf
<VirtualHost *:80>
    ServerName mySite.local
    ServerAlias www.mySite.local
    DocumentRoot /var/www/html/mySite

    <Directory /var/www/html/mySite>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/mySite_error.log
    CustomLog ${APACHE_LOG_DIR}/mySite_access.log combined
</VirtualHost>
```

> `AllowOverride All` is important for Joomla, since it relies on `.htaccess` for SEF (search engine friendly) URLs.

## 2. Enable the Site and Required Modules

```bash
sudo a2ensite mySite.conf
sudo a2enmod rewrite
```

## 3. Add the Local Domain to Hosts File

```bash
sudo subl /etc/hosts
```

Add this line:

```
127.0.0.1   mySite.local www.mySite.local
```

## 4. Restart Apache

```bash
sudo systemctl restart apache2
```

## 5. Test It

Open your browser and go to:

```
http://mySite.local
```

You should see your Joomla site load.

## 6. Update Joomla's Site URL

Joomla stores its base URL in the database. If you switch from `http://127.0.0.1/mySite` to `http://mySite.local`, update it:

1. Log in to Joomla admin at `http://mySite.local/administrator`
2. Go to **System → Global Configuration**
3. Update the **Site URL** field (if present) — or check `configuration.php` in your Joomla root:

```bash
subl /var/www/html/mySite/configuration.php
```

Look for and update:

```php
public $live_site = 'http://mySite.local';
```

## Troubleshooting

| Issue | Fix |
|---|---|
| 403 Forbidden | Check folder permissions: `sudo chown -R www-data:www-data /var/www/html/mySite` |
| .htaccess not working | Confirm `AllowOverride All` and `a2enmod rewrite` was run |
| Domain doesn't resolve | Double-check `/etc/hosts` entry and clear browser DNS cache |
| Apache won't restart | Run `sudo apache2ctl configtest` to find syntax errors |

## Notes

- If you're running multiple Joomla sites, repeat this process with a unique `ServerName` and `.conf` file per site.
- Consider adding this vhost to Nginx Proxy Manager later if you want SSL or a reverse proxy in front of it.
