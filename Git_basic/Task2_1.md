# Task 2.1 Базовая работа с git-репозиториями
### 1. Установить консольный git-клиент. Настроить git-клиент, указав параметры user.name и user.email, соответствующие вашим данным. Создать репозиторий devops-task1 с помощью git init. Создать файл nginx.conf, содержащий конфигурацию nginx (базовую, которая появляется после установки пакета в linux /etc/nginx/nginx.conf). 
```powershell
  git config --global user.name "Your Name"
  git config --global user.email "your.email@example.com"

  # Create and initialize repo
  mkdir devops-task1
  cd devops-task1
  git init

  # Create nginx.conf
    @"
    user www-data;
    worker_processes auto;
    pid /run/nginx.pid;
    include /etc/nginx/modules-enabled/*.conf;

    events {
        worker_connections 768;
    }

    http {
        sendfile on;
        include /etc/nginx/mime.types;
        default_type application/octet-stream;
        access_log /var/log/nginx/access.log;
        error_log /var/log/nginx/error.log;
    }
    "@ | Set-Content -Path "nginx.conf" -Encoding UTF8
```
### 2. Создать файл README.md с описанием того, что находится в репозитории (пример: "В данном репозитории находится дефолтный конфигурационный файл nginx"). Добавить файл README.md в репозиторий с помощью команды git commit. Добавить файл nginx.conf в репозиторий с помощью команды git commit.
```powershell
  # Create README
  echo "This repository contains the default nginx configuration file." > README.md
  # Stage and commit
  git add README.md
  git commit -m "Add README file"
  git add nginx.conf
  git commit -m "Add nginx.conf with default configuration"

```
### 3. С помощью команды git log проверить, отображаются ли в истории два коммита. С помощью команды git status проверить, что в данной папке не осталось больше файлов/папок, которые не добавлены в репозиторий.
```powershell
  git log --oneline
  # Should show 2 commits
  git status
  # Should say 'nothing to commit, working tree clean'
```
### 4. Зарегистрироваться в двух публичных хостингах git-репозиториев bitbucket и github. Доступ к devops-gitlab.inno.ws ты должен был получить в начале курса. Если возникли какие-то проблемы с доступом, свяжись с ментором.
### 5. Создать на каждом из хостингов репозиторий devops-task1; bitbucket и github - публичные (public) репозитории, devops-gitlab.inno.ws - внутренний (internal). В репозиторий, созданный в первом задании, добавить 3 удаленных сервера с помощью git remote add. Загрузить репозиторий во все git-хостинги с помощью git push.
```powershell
  # Add all 3 remotes
  git remote add github https://github.com/yourusername/devops-task1.git
  git remote add gitlab https://devops-gitlab.inno.ws/yourusername/devops-task1.git
  # Push to all
  git push github master
  git push gitlab master
```
### 6. Клонировать репозиторий. Переключиться на 3-й от начала коммит (e8b3ec06b). Просмотреть содержимое файла deleted.txt.
```powershell
  git clone https://github.com/yourusername/devops-task1.git cloned-repo
  cd cloned-repo
  git log --oneline
  # Find 3rd commit hash (e.g., 08574a8). Switches to the commit with hash 08574a8, this puts the repo in a detached HEAD state (you’re not on a branch anymore).
  git checkout 08574a8
```
### 7. Проверить, что все созданные репозитории публичные, кроме репозитория на devops-gitlab.inno.ws.