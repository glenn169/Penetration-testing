# IMPORTANT!!
1. replace the exising `/zero-health/docker-compose.yml` with the given `docker-compose.yml` file
2. Run `docker compose down` and `docker compose up -d`
   
# Make sure to add this
1. Paste the given `zerohealth.lab.conf` file to `/etc/nginx/sites-available/`
2. ```
   sudo ln -s /etc/nginx/sites-available/zerohealth.lab.conf /etc/nginx/sites-enabled/
   sudo nginx -t
   systemctl reload nginx
