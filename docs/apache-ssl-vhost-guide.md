# Apache SSL Virtual Host Setup Guide

A step-by-step guide to creating a locally trusted HTTPS virtual host on Linux Mint (Apache 2), using **mkcert** to generate certificates that browsers accept without warnings.

---

## Prerequisites

- Apache 2 installed and running
- A site already served from a local directory (e.g. `/var/www/html/mysite`)
- `sudo` access

---

## Step 1 — Install mkcert

mkcert is a tool that creates locally trusted SSL certificates without browser warnings.

```bash
sudo apt install libnss3-tools
sudo apt install mkcert
```

---

## Step 2 — Install the Local Certificate Authority

Run this **as your regular user** (not sudo):

```bash
mkcert -install
```

This creates a private Certificate Authority (CA) on your machine and registers it with your system trust store. Certificates signed by this CA will be trusted by your browser.

> **Firefox note:** Firefox maintains its own certificate store. Close Firefox before running `mkcert -install`, then reopen it. If warnings still appear, see the [Firefox Troubleshooting](#firefox-troubleshooting) section below.

---

## Step 3 — Generate the Certificate

Create a dedicated folder for your SSL certificates and generate a cert for your local domain:

```bash
sudo mkdir -p /etc/apache2/ssl
cd /etc/apache2/ssl
sudo mkcert mysite.local www.mysite.local
```

Replace `mysite.local` with your own local domain name.

This produces two files:

| File | Purpose |
|------|---------|
| `mysite.local+1.pem` | The SSL certificate |
| `mysite.local+1-key.pem` | The private key |

---

## Step 4 — Enable Apache SSL Module

```bash
sudo a2enmod ssl
sudo a2enmod rewrite
```

`mod_rewrite` is required for CMS platforms (Joomla, WordPress, etc.) to handle SEF/clean URLs via `.htaccess`.

---

## Step 5 — Create the Virtual Host Config

Create a config file for your site:

```bash
sudo nano /etc/apache2/sites-available/mysite.local.conf
```
Paste the following, replacing `mysite` and `mysite.local` with your own values:


```apache
# Redirect HTTP → HTTPS
<VirtualHost *:80>
    ServerName mysite.local
    ServerAlias www.mysite.local
    Redirect permanent / https://mysite.local/
</VirtualHost>

# HTTPS
<VirtualHost *:443>
    ServerName mysite.local
    ServerAlias www.mysite.local
    DocumentRoot /var/www/html/mysite

    SSLEngine on
    SSLCertificateFile    /etc/apache2/ssl/mysite.local+1.pem
    SSLCertificateKeyFile /etc/apache2/ssl/mysite.local+1-key.pem

    <Directory /var/www/html/mysite>
        Options FollowSymLinks MultiViews
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/mysite_error.log
    CustomLog ${APACHE_LOG_DIR}/mysite_access.log combined
</VirtualHost>
```

> `AllowOverride All` is required for CMS platforms that rely on `.htaccess` for URL routing.

---

## Step 6 — Enable the Site

```bash
sudo a2ensite mysite.local.conf
```

---

## Step 7 — Add the Hostname to /etc/hosts

This tells your machine to resolve `mysite.local` to itself instead of looking it up on the internet:

```bash
sudo nano /etc/hosts
```

Add this line:

```
127.0.0.1   mysite.local
```

---

## Step 8 — Reload Apache

```bash
sudo systemctl reload apache2
```

Your site is now accessible at `https://mysite.local` with a valid, trusted certificate.

---

## Firefox Troubleshooting

If Firefox still shows a security warning after completing the steps above:

1. Open Firefox and go to **Settings → Privacy & Security → View Certificates**
2. Click the **Authorities** tab → **Import**
3. Find the mkcert CA root with:
   ```bash
   mkcert -CAROOT
   ```
4. Navigate to that folder and import `rootCA.pem`
5. Check both trust options:
   - ✅ Trust this CA to identify websites
   - ✅ Trust this CA to identify email users
6. Restart Firefox

---

## CMS Configuration (Joomla / WordPress)

After switching to HTTPS, update your CMS base URL to avoid broken links or redirect loops.

**Joomla:** Go to **Global Configuration → Server** and set the site URL to `https://mysite.local`

**WordPress:** Go to **Settings → General** and update both the WordPress Address and Site Address to `https://mysite.local`

---

## Security Notes

mkcert is safe for local development. Key points:

- The CA and certificates never leave your machine
- `.local` domains are not publicly routable — no external access is possible
- The only sensitive file is `rootCA-key.pem` in your mkcert folder. Verify its permissions are owner-only:
  ```bash
  ls -la $(mkcert -CAROOT)
  ```
  The key file should show `-rw-------` (600). If not, fix it:
  ```bash
  chmod 600 $(mkcert -CAROOT)/rootCA-key.pem
  ```

---

## Quick Reference

| Command | Purpose |
|--------|---------|
| `mkcert -install` | Install local CA (run once, as regular user) |
| `mkcert -CAROOT` | Show the CA root directory |
| `sudo a2ensite mysite.local.conf` | Enable a virtual host |
| `sudo a2dissite mysite.local.conf` | Disable a virtual host |
| `sudo a2enmod ssl` | Enable SSL module |
| `sudo a2enmod rewrite` | Enable rewrite module |
| `sudo systemctl reload apache2` | Apply config changes |
| `sudo apache2ctl configtest` | Test config for errors before reloading |
