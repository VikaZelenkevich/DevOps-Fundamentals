# Task 3.3 Basics. Запуск команд внутри контейнера
### 1. Скачай конфигурационный файл nginx. Запусти контейнер со следующими параметрами: должен работать в фоне, слушает на хосте 127.0.0.1:8890, имеет имя inno-dkr-03, должен пробрасывать скачанный вами конфигурационный файл внутрь контейнера как основной конфигурационный файл, образ - nginx:stable.
```powershell
  # Create nginx.cong and add needed configuration
  # Run container, -v Bind mount the nginx.conf file from your current directory into the container’s /etc/nginx/nginx.conf, where ro: (read-only).
  docker run -d -p 127.0.0.1:8890:80 --name inno-dkr-03 -v ${PWD}\nginx.conf:/etc/nginx/nginx.conf:ro nginx:stable
```
### 2. Проверь работу, обратившись к 127.0.0.1:8890, - в ответ должно возвращать строку Welcome to the training program Innowise: Docker Выведи список запущенных контейнеров - контейнер должен быть запущен. Выполни команду docker exec -ti inno-dkr-03 md5sum /etc/nginx/nginx.conf
```powershell
  curl http://127.0.0.1:8890
  # Check running containers
  docker ps
  # Executes a command inside a running Docker container. This is the Docker command used to run a new command in a running container. -t (or --tty): Allocates a pseudo-TTY (terminal) for the container. This is necessary if you want to see formatted output, like when running interactive shell commands. -i (or --interactive): Keeps STDIN open even if not attached. This allows you to provide input to the command running inside the container 
  docker exec -ti inno-dkr-03 md5sum /etc/nginx/nginx.conf 
  # I got this: 70cf706bb95a6fe69fce0ffd6fbd8601
```
### 3. Скачай новый конфигурационный файл nginx. Измени проброшенный конфигурационный файл, размещенный на хостовой системе, на новый. Проведи эксперименты с различными способами изменения: через команду cp, через vim, nano. За vim +2 балла в карму. Выполни reload nginx без остановки контейнера при помощи команды exec. Проверь работу, обратившись к 127.0.0.1:8890, - в ответ должно возвращать строку Welcome to the training program Innowise: Docker! Again!
```powershell
  # In bash terminal I change nginx.conf: vim nginx.conf, add this phrase Docker! Again!
  # Reload nginx inside the container without stopping it
  docker exec inno-dkr-03 nginx -s reload
  # Check the updated response:
  curl http://127.0.0.1:8890
```
### 4. Выполни команду docker exec -ti inno-dkr-03 md5sum /etc/nginx/nginx.conf (вывод в консоли сохраняем). Сравни результаты изменения файла nginx.conf разными командами. Найди информацию, почему в некоторых случаях изменение не отображалось в контейнере. Выведи список запущенных контейнеров - контейнер должен быть запущен.
```powershell
  docker exec -ti inno-dkr-03 md5sum /etc/nginx/nginx.conf 
  # I got this: c58ad86ba7664eb8917dea2382a81cc0
  # Check running containers
  docker ps
```