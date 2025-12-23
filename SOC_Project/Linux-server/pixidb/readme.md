# IMPORTANT!!
1. replace this existing `docker-compose.yml` file with the given `docker-compose.yml` file from pixidb
2. run `docker compose down` and `docker compose up -d`


# Make sure to add this
1. Paste the given `pixi.lab.conf` file to `/etc/nginx/sites-available/`
2. ```
   sudo ln -s /etc/nginx/sites-available/pixi.lab.conf /etc/nginx/sites-enabled/
   sudo nginx -t
   systemctl reload nginx
