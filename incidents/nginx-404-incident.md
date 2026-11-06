# Incident Report: Nginx HTTP 404 Error

## 1. Incident Summary

- **Service:** Nginx Web Server
- **Environment:** Ubuntu 24.04 (WSL2)
- **Incident Type:** Website availability / configuration issue
- **Observed HTTP Status:** 404 Not Found
- **Resolution:** Restored the correct document root configuration

## 2. Symptoms

The custom website `infrastructure-lab.local` returned:

`HTTP/1.1 404 Not Found`

The Nginx service remained active, but the expected website content was unavailable.

## 3. Investigation

The following checks were performed:

- Inspected the Nginx website configuration.
- Checked the configured document root.
- Verified whether the configured directory existed.
- Reviewed the Nginx error log.

## 4. Root Cause

The Nginx server block contained an incorrect document root:

`/var/www/wrong-directory`

The directory did not exist. The intended website files were located in:

`/var/www/infrastructure-lab`

## 5. Corrective Action

- Restored the previous working Nginx configuration from backup.
- Reloaded the Nginx service.
- Tested the website again using curl.

## 6. Verification

The website returned:

`HTTP/1.1 200 OK`

The expected HTML content was successfully served.

## 7. Lessons Learned

- Verify document roots when investigating HTTP 404 errors.
- Keep backups before modifying service configurations.
- Validate configurations before applying changes.
- Verify service recovery using HTTP requests.
- Distinguish between a running web server and an accessible website.

## 8. Skills Demonstrated

Linux administration, Nginx configuration, service management, log inspection, HTTP troubleshooting, configuration backup and recovery.