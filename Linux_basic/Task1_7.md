# Task 1.7 Nginx. Round Robin
### 1. Создать сервер t2.micro, он должен иметь публичный ip и доступ в интернет. Разрешить http трафик в security group, ассоциированной с данным сервером.  
```
In VirtualBox, create four Ubuntu virtual machines: Ubuntu-VM, Ubuntu-VM2, load-balancer, backup-server. 
For each VM, go to Settings > Network. Attach the first adapter to Bridged Adapter to allow the VM to have a public IP on your local network. Start the VMs and ensure they have internet access by running:
ping -c 3 google.com
```
### 2. Установить Nginx, напиши конфигурационный файл, который будет раздавать html-страницу blue.html. Проверить работоспособность(выдает ли он нужную страницу).
```bash
  # In first VM (Ubuntu-VM) install Nginx :
  sudo apt update
  sudo apt install nginx -y
  # Create blue.html
  echo '<html><body style="background-color:blue;"><h1>Blue Page</h1></body></html>' | sudo tee /var/www/html/blue.html
  # Configure Nginx to Serve blue.html, edit the default Nginx configuration:
  sudo nano /etc/nginx/sites-available/default
  # Modify the location / block:
```
```
location / {
    root /var/www/html;
    index blue.html;
}
```
```bash
  sudo systemctl restart nginx
  # After that try to access http://<server-ip> from a browser
```
### 3. Создать еще два сервера t2.micro, на одном настроить раздачу страницы yellow.html, на втором настроить балансировщик между yellow и blue. Балансировщик должен направлять запрос на сервер с меньшим количеством подключений. По итогу должно получиться так, что при каждом новом подключении цвет заднего фона должен меняться.
```bash
  # In second VM (Ubuntu-VM2) install Nginx :
  sudo apt update
  sudo apt install nginx -y
  # Create yellow.html
  echo '<html><body style="background-color:yellow;"><h1>Yellow Page</h1></body></html>' | sudo tee /var/www/html/yellow.html
  # Configure Nginx to Serve yellow.html, edit the default Nginx configuration:
  sudo nano /etc/nginx/sites-available/default
  # Modify the location / block:
```
```
location / {
    root /var/www/html;
    index yellow.html;
}
```
```bash
  sudo systemctl restart nginx
  # After that try to access http://<server-ip> from a browser
  # In load-balancer server
  sudo apt update
  sudo apt install nginx -y
  # Create a new configuration file:
  sudo nano /etc/nginx/conf.d/load-balancer.conf
  # Add the following configuration:
```
```
upstream backend {
    least_conn;
    server <Ubuntu-VM-bridged-ip>;
    server <Ubuntu-VM2-bridged-ip>;
}

server {
    listen 80;

    location / {
        proxy_pass http://backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
```bash
  sudo systemctl restart nginx
  # After that try to access http://<load-balancer-ip> from a browser
  # I got 504 gateway error so I ping the backend servers (from load-balancer server), than check that no firewall rules are blocking traffic between the load balancer and backend servers, so in backend servers: sudo ufw status, Allow HTTP Traffic: sudo ufw allow http and than it worked
```
### 4. Добавить в конфигурацию вывод логов, который будет показывать, куда проксируется запрос клиента. 
```bash
  sudo nano /etc/nginx/nginx.conf
  # To http block add this:
```
```
    log_format backend '$remote_addr - $remote_user [$time_local] '
                           '"$request" $status $body_bytes_sent '
                           '"$http_referer" "$http_user_agent" "$http_add_x_forwarded_for"';
    access_log /spool/logs/nginx-access.log compression;

```
```bash
  # View the Access Log::
  sudo tail -f /var/log/nginx/access.log
```
### 5. Создать еще один сервер с сайтом и добавить его в конфигурацию как backup сервер.
```bash
  # In backup-server install Nginx :
  sudo apt update
  sudo apt install nginx -y
  # Create backup.html:
  echo '<html><body style="background-color:gray;"><h1>Backup Page</h1></body></html>' | sudo tee /var/www/html/backup.html
  # Configure Nginx to Serve backup.html, edit the default Nginx configuration:
  sudo nano /etc/nginx/sites-available/default
  # Modify the location / block:
```
```
location / {
    root /var/www/html;
    index backup.html;
}
```
```bash
  sudo systemctl restart nginx
  # Update Load Balancer Configuration. On load-balancer, edit the load-balancer.conf file:
  sudo nano /etc/nginx/conf.d/load-balancer.conf
  # Modify the upstream block to include the backup server:
```
```
    upstream backend {
        least_conn;
        server <Ubuntu-VM-bridged-ip>;
        server <Ubuntu-VM2-bridged-ip>;
        server <backup-server-ip> backup;
    }
```
```bash
  # Replace <backup-server-ip> with the actual IP address of your backup-server. Save and exit the editor.
  # Restart Nginx:
  sudo systemctl restart nginx
  # Test Backup Functionality. Stop Nginx on both Ubuntu-VM and Ubuntu-VM2:
  sudo systemctl stop nginx
  # Access http://<load-balancer-ip> from a browser. You should see the backup page served by backup-server.
```

### 6. Выгрузить конфиги в личный репозиторий на devops-gitlab.inno.ws.