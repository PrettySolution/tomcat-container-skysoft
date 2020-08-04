1. [install tomcat on ubuntu](https://www.digitalocean.com/community/tutorials/install-tomcat-9-ubuntu-1804-ru)  
* sudo apt update  
* sudo apt install default-jdk
* sudo groupadd tomcat
* sudo useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat
* cd /tmp
* curl -O https://www2.apache.paket.ua/tomcat/tomcat-9/v9.0.37/bin/apache-tomcat-9.0.37.tar.gz
* sudo mkdir /opt/tomcat
* sudo tar xzvf apache-tomcat-*tar.gz -C /opt/tomcat --strip-components=1
* cd /opt/tomcat
* sudo chgrp -R tomcat /opt/tomcat
* sudo chmod -R g+r conf
* sudo chmod g+x conf
* sudo chown -R tomcat webapps/ work/ temp/ logs/
* sudo update-java-alternatives -l
* sudo nano /etc/systemd/system/tomcat.service
```text
[Unit]
Description=Apache Tomcat Web Application Container
After=network.target

[Service]
Type=forking

Environment=JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64
Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
Environment=CATALINA_HOME=/opt/tomcat
Environment=CATALINA_BASE=/opt/tomcat
Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

User=tomcat
Group=tomcat
UMask=0007
RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
```
* sudo systemctl daemon-reload
* sudo systemctl enable tomcat.service

1. [Install nginx on ubuntu](https://www.digitalocean.com/community/tutorials/nginx-ubuntu-18-04-ru)  
sudo apt install nginx
nano /etc/nginx/conf.d/tomcat.conf

```text
server {
        listen 80;
        server_name events.skysoft.cloud photos.skysoft.cloud;

        location ~ /.well-known/acme-challenge {
          allow all;
          root /var/www/html;
        }

        location / {
                rewrite ^ https://$host$request_uri? permanent;
        }
}
```


1. [certbot ubuntu nginx](https://www.digitalocean.com/community/tutorials/nginx-let-s-encrypt-ubuntu-18-04-ru)
sudo add-apt-repository ppa:certbot/certbot
sudo apt install python-certbot-nginx

sudo /usr/bin/certbot --nginx -d events.skysoft.cloud -d photos.skysoft.cloud