*************************************************
sudo nano /etc/nginx/sites-available/tennis_club
server {
    listen 80;
    server_name 104.236.100.55;

    location = /favicon.ico { access_log off; log_not_found off; } 
    location /static/ {
        alias /var/www/tennis-club/productionfiles/;
    }

    location /media/ {
        alias /var/www/tennis-club/media/;
    }

    location / {
        include proxy_params;  
        proxy_pass http://unix:/run/gunicorn.sock;
    }
}

**************************************************
sudo nano /etc/systemd/system/gunicorn.socket
[Unit]
Description=gunicorn socket

[Socket]
ListenStream=/run/gunicorn.sock

[Install]
WantedBy=sockets.target

**************************************************
sudo nano /etc/systemd/system/gunicorn.service  
[Unit]
Description=gunicorn daemon
Requires=gunicorn.socket
After=network.target

[Service]
User=root
Group=www-data
WorkingDirectory=/var/www/tennis-club
ExecStart=/var/www/tennis-club/.venv/bin/gunicorn --access-logfile - --workers 3 --bind unix:/var/www/tennis-club/tennis-club.sock tennis_club.wsgi:application

[Install]
WantedBy=multi-user.target

