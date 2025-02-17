# How to Attach Your Domain to Your Web App

## Overview
This guide explains how to link your domain `joselrnz.com` to your web application hosted on `1.2.3.4`.

## Prerequisites
- A registered domain (`joselrnz.com`).
- Access to your domain registrar's DNS settings (GoDaddy, Cloudflare, Namecheap, etc.).
- A web server (e.g., Nginx, Apache, IIS) running on `1.2.3.4`.

---

## Step 1: Configure DNS Records
You need to point your domain to your server’s IP address.

### Add an A Record
1. **Log in to your DNS provider** (e.g., Cloudflare, Namecheap, GoDaddy).
2. **Find the DNS settings** for `joselrnz.com`.
3. **Create an A record**:
   - **Host:** `@` (this represents `joselrnz.com`)
   - **Type:** `A`
   - **Value:** `1.2.3.4`
   - **TTL:** `Auto` or `300` seconds
4. **Save the changes**.

### Add a CNAME Record for `www`
To allow `www.joselrnz.com` to also point to your site:
1. **Create a CNAME record**:
   - **Host:** `www`
   - **Type:** `CNAME`
   - **Value:** `joselrnz.com`
   - **TTL:** `Auto`
2. **Save the changes**.

✅ **Verify:** Use `nslookup joselrnz.com` or `ping joselrnz.com` to check if the DNS is resolving correctly.

---

## Step 2: Configure Your Web Server
### **For Nginx**
Edit the configuration file (e.g., `/etc/nginx/sites-available/joselrnz.com`):
```nginx
server {
    listen 80;
    server_name joselrnz.com www.joselrnz.com;
    root /var/www/html;
    index index.html;
}
```
Then reload Nginx:
```sh
sudo systemctl restart nginx
```

### **For Apache**
Edit the virtual host configuration (e.g., `/etc/apache2/sites-available/joselrnz.com.conf`):
```apache
<VirtualHost *:80>
    ServerName joselrnz.com
    ServerAlias www.joselrnz.com
    DocumentRoot /var/www/html
</VirtualHost>
```
Then reload Apache:
```sh
sudo systemctl restart apache2
```

✅ **Verify:** Try accessing `http://joselrnz.com` in your browser.

---

## Step 3: Enable HTTPS (SSL Certificate)
To enable SSL for secure HTTPS access, use Let's Encrypt with Certbot:

### **Install Certbot** (for Nginx & Apache):
```sh
sudo apt update
sudo apt install certbot python3-certbot-nginx -y  # For Nginx
sudo apt install certbot python3-certbot-apache -y  # For Apache
```

### **Generate SSL Certificate:**
```sh
sudo certbot --nginx -d joselrnz.com -d www.joselrnz.com  # For Nginx
sudo certbot --apache -d joselrnz.com -d www.joselrnz.com  # For Apache
```

### **Renew SSL Automatically:**
```sh
sudo certbot renew --dry-run
```
✅ **Verify:** Try accessing `https://joselrnz.com`.

---

## Step 4: Redirect HTTP to HTTPS (Optional)
To force HTTPS, add the following to your Nginx configuration:
```nginx
server {
    listen 80;
    server_name joselrnz.com www.joselrnz.com;
    return 301 https://$host$request_uri;
}
```
For Apache, update `.htaccess`:
```apache
RewriteEngine On
RewriteCond %{HTTPS} !=on
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```
Reload the web server:
```sh
sudo systemctl restart nginx  # or apache2
```

---

## Step 5: Test Your Setup
### **Check DNS Resolution**
Run:
```sh
nslookup joselrnz.com
ping joselrnz.com
```
It should return `1.2.3.4`.

### **Check Web Access**
- Open `http://joselrnz.com` → Should redirect to `https://joselrnz.com`.
- Open `https://joselrnz.com` → Should load without SSL errors.

---

## Troubleshooting
| Issue | Solution |
|--------|----------|
| Website not loading | Check if the server is running (`systemctl status nginx` or `systemctl status apache2`). |
| SSL certificate not working | Run `certbot renew` and restart the web server. |
| DNS not resolving | Wait for DNS propagation (can take a few hours). Use `dig joselrnz.com` to check DNS. |
| HTTP not redirecting to HTTPS | Ensure you added the redirect rules correctly in Nginx or Apache. |

---

## Conclusion
By following these steps, you have successfully attached `joselrnz.com` to your web app running on `1.2.3.4`, configured SSL, and set up a secure web server.

Let me know if you need further help! 🚀

