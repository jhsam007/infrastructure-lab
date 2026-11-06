# Nginx Website Deployment Guide

## 1. Environment

- Operating System: Ubuntu 24.04 LTS (WSL2)
- Web Server: Nginx 1.24.0
- Website hostname: `infrastructure-lab.local`
- HTTP port: `80`

## 2. Project Files

The repository contains:

- `web/index.html` — website content
- `nginx/infrastructure-lab.conf` — Nginx server-block configuration

## 3. Install Nginx

```bash
sudo apt update
sudo apt install nginx -y
```

## 4. Deploy Website Files

Create the document root:

```bash
sudo mkdir -p /var/www/infrastructure-lab
```

Copy the website:

```bash
sudo cp web/index.html /var/www/infrastructure-lab/
```

Set ownership and permissions:

```bash
sudo chown -R www-data:www-data /var/www/infrastructure-lab
sudo chmod -R 755 /var/www/infrastructure-lab
```

## 5. Configure Nginx

Copy the site configuration:

```bash
sudo cp nginx/infrastructure-lab.conf /etc/nginx/sites-available/infrastructure-lab
```

Enable the website:

```bash
sudo ln -s /etc/nginx/sites-available/infrastructure-lab /etc/nginx/sites-enabled/
```

Validate the configuration:

```bash
sudo nginx -t
```

Reload Nginx:

```bash
sudo systemctl reload nginx
```

## 6. Configure Local Hostname

Add the hostname entry if it does not already exist:

```bash
if ! grep -qxF '127.0.0.1 infrastructure-lab.local' /etc/hosts; then
    echo '127.0.0.1 infrastructure-lab.local' | sudo tee -a /etc/hosts
fi
```

Verify hostname resolution:

```bash
getent hosts infrastructure-lab.local
```

This maps the local hostname to the loopback address in the Ubuntu environment.

## 7. Verify Deployment

Check the HTTP response:

```bash
curl -I http://infrastructure-lab.local
```

Expected response:

```text
HTTP/1.1 200 OK
```

View website content:

```bash
curl http://infrastructure-lab.local
```

## 8. Troubleshooting

If the website returns HTTP 404:

1. Check the Nginx site configuration.
2. Verify the document root exists.
3. Confirm `index.html` is present.
4. Test the Nginx configuration using `sudo nginx -t`.
5. Inspect logs:

```bash
sudo tail -n 30 /var/log/nginx/error.log
sudo tail -n 30 /var/log/nginx/access.log
```

## 9. Important Notes

- These instructions target Ubuntu 24.04 running under WSL2.
- The hostname entry is local to the Ubuntu environment.
- Port 80 must be available.
- This is a local practice deployment, not a public production hosting setup.