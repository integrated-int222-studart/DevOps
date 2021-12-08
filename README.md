# NginX Reverse-Proxy+SSL Step
## Tools
* docker container
* docker-compose
* nginx image
* certbot image
* domain name
* email for register certificate
## Step
   1. clone this git repo to work directory 
   2. download certbot setting file for SSL setting
   ```
     mkdir -p "certbot/conf"
     curl -s https://raw.githubusercontent.com/certbot/certbot/master/certbot-nginx/certbot_nginx/_internal/tls_configs/options-ssl-nginx.conf > "certbot/conf/options-ssl-nginx.conf"
     curl -s https://raw.githubusercontent.com/certbot/certbot/master/certbot/certbot/ssl-dhparams.pem > "certbot/conf/ssl-dhparams.pem"
   ```
   3. install HTTP site for register certificate by comment syntax server listen 443 in config/reverse.conf
   4. run docker container by docker-compose file
   ```
      docker-compose up -d
   ```
   5. run command for register certificate
   ```
      docker exec -it {container_name} certbot certonly --webroot -w /var/www/certbot -d {domain}
   ```
   * check path of certificate
   ```
      docker exec -it {certbot_container_name} certbot certificates
   ```
   6. uncomment server listen 443 in config/reverse.conf and swicth comment in line 39,40
   7. save and reload nginx for execute nginx config
   ```
     docker exec -it {revere_proxy_container_name } nginx -t
     docker exec -it {revere_proxy_container_name } nginx -s reload
   ```
> ref : https://prudchayapalee.medium.com/%E0%B8%97%E0%B8%B3-ssl-https-%E0%B9%82%E0%B8%94%E0%B8%A2%E0%B9%83%E0%B8%8A%E0%B9%89-lets-encrypt-cert-bot-%E0%B8%9A%E0%B8%99-nginx-%E0%B9%83%E0%B8%99%E0%B9%81%E0%B8%9A%E0%B8%9A%E0%B8%89%E0%B8%9A%E0%B8%B1%E0%B8%9A-docker-auto-renew-certificate-bc573e127f28
