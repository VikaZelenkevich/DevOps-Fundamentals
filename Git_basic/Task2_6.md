# Task 2.6 Submodules
### 1. Создай пустой репозиторий. Добавь в него Wordpress как сабмодуль с именем wordpress. Далее зайди в этот сабмодуль и сделай git fetch -a, просмотри все имеющиеся тэги и верни сабмодуль к состоянию с тэгом 3.4.2. Сделай коммит основного репозитория, чтобы зафиксировать версию сабмодуля.
```powershell
  mkdir wordpress-submodule-project
  cd wordpress-submodule-project
  git init
  # Add wordpress as submodule
  git submodule add https://github.com/WordPress/WordPress.git wordpress
  # Go into the submodule and fetch tags
  cd wordpress
  git fetch --all --tags
  git tag
  # Checkout tag 3.4.2
  git checkout tags/3.4.2
  cd ..
  git add wordpress
  git commit -m "Added WordPress submodule at version 3.4.2"
```
### 2. Скопируй из сабмодуля в свой репозиторий следующие файлы, а также очисти от ненужных:
- cp wordpress/index.php index.php
- cp wordpress/wp-config-sample.php wp-config.php
- cp -Rf wordpress/wp-content .
```powershell
  cp wordpress/index.php index.php
  cp wordpress/wp-config-sample.php wp-config.php
  cp -Recurse -Force wordpress/wp-content wp-content   
```
### 3. rm -Rf wp-content/plugins/hello.php wp-content/themes/twentyten.
```powershell
  Remove-Item -Force wp-content\plugins\hello.php
  Remove-Item -Recurse -Force wp-content\themes\twentyten 
```
### 4. Теперь нам нужно подшаманить наши перенесенные файлы так, чтобы они взаимодействовали с сабмодулем. Измени строку 17 в index.php на require('wordpress/wp-blog-header.php').
```powershell
  (Get-Content index.php) -replace "require\(.+\)", "require('wordpress/wp-blog-header.php');" | Set-Content index.php 
```
### 5. Зафиксируй все изменения коммитом и запушь репозиторий на devops-gitlab.inno.ws.
```powershell
  git remote add origin https://devops-gitlab.inno.ws/YOUR_USERNAME/YOUR_REPO.git
  git push -u origin master
```

