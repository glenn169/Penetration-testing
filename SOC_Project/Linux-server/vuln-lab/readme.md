# IMPORTANT ! 
1. make sure to replace the given docker-compose.yml with the existing docker-compose.yml
2. `docker compose down`
3. `docker compose up -d`


# Make sure to add this
1. Paste the given `vuln-lab.conf` file to `/etc/nginx/sites-available/`
2. ```
   sudo ln -s /etc/nginx/sites-available/vuln-lab.conf /etc/nginx/sites-enabled/
   sudo nginx -t
   systemctl reload nginx
