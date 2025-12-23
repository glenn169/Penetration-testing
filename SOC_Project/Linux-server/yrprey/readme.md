# IMPORTANT!!
1. Replace your existing docker file from `/path/to/yrprey/.docker/dev` and replace `app.yml` with given `docker-compose.yml`
2. run `docker compose down` and `docker compose up -d`

# Make sure to add this
1. Paste the given `yrprey.lab.conf` file to `/etc/nginx/sites-available/`
2. ```
   sudo ln -s /etc/nginx/sites-available/yrprey.lab.conf /etc/nginx/sites-enabled/
   sudo nginx -t
   systemctl reload nginx
