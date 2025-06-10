#  Task 2.2 Бранчевание
### 1. В репозитории devops-task1, загруженном на хостинг в предыдущем задании, создай с помощью git checkout и git branch две новые ветки: develop и feature/new-site. 
```powershell
  # Create branch and switch to it
  git checkout -b develop
  # Now create feature/new-site branch
  git branch feature/new-site
```
### 2. В ветке develop измени настройки в nginx.conf: замени worker_connections на 16384 и закоммить изменения. В ветке develop измени nginx.conf, включи сжатие ответа для типа application/json и закоммить изменения.
```powershell
  # Check in which branch you are
  git branch
  # In not in develop use:
  git checkout develop
  # Change worker_connections to 16384:
  
  (Get-Content "D:\DevOps_Fundamentals\devops-task1\nginx.conf") -replace 'worker_connections\s+\d+;', 'worker_connections 16384;' | Set-Content "D:\DevOps_Fundamentals\devops-task1\nginx.conf"
  git add nginx.conf
  git commit -m "Updated worker_connections to 16384"
  # Add these lines inside the http block of nginx.conf:
  gzip on;
  gzip_types application/json;
  # Then in terminal:
  git add nginx.conf
  git commit -m "Enable gzip compression for application/json"
```
### 3. В ветке feature/new-site добавь файл conf.d/mysite.domain.com.conf с базовым описанием статического сайта и закоммить изменения.
```powershell
  # Switch to feature/new-site branch
  git checkout feature/new-site
  mkdir -p conf.d
  # Create the config file with basic NGINX static site configuration:
  @"
  server {
    listen 80;
    server_name mysite.domain.com;

    location / {
        root /var/www/mysite;
        index index.html;
    }
  }
  "@ | Set-Content conf.d\mysite.domain.com.conf
```
```powershell
  # Stage and commit the file:
  git add conf.d/mysite.domain.com.conf
  git commit -m "Add static site config for mysite.domain.com"
```
### 4. Добавь легковесный тег v0.1 на последний коммит в  ветке{{develop}} (где изменял nginx.conf). 
```powershell
  # Switch to branch
  git checkout develop
  git tag v0.1
```
### 5. Добавь файл .gitignore в ветку feature/new-site. Файл gitignore должен быть составлен таким образом, чтобы исключать из репозитория папку tmp и все ее содержимое. Создай локально папку tmp с несколькими файлами в ней, закоммить изменения.
```powershell
  echo "tmp/" > .gitignore
  git add .gitignore
  git commit -m "Add .gitignore to exclude tmp directory"
  # Now create a local tmp folder and files in it: 
  mkdir tmp
  New-Item -ItemType File -Path tmp/file1.log
  New-Item -ItemType File -Path tmp/debug.txt
```
### 6. Загрузи обе ветки на gitlab. Загрузи тег на хостинг https://devops-gitlab.inno.ws с помощью git push и проверь его наличие. Присутствует ли директория tmp в ветке feature/new-site?
```powershell
  # Push develop and its tag
  git push gitlab develop
  git push gitlab v0.1

  # Push feature branch
  git push gitlab feature/new-site
```