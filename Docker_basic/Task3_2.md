# Task 3.2 Basics. Флаги запуска
### 1. Скачай конфигурационный файл nginx. Запусти контейнер со следующими параметрами: a. должен работать в фоне, b. слушает на хосте 127.0.0.1:8889, c. имеет имя inno-dkr-02, d. должен пробрасывать скачанный тобой конфигурационный файл nginx внутрь контейнера как основной конфигурационный файл, e. образ - nginx:stable.
```powershell
  # Create nginx.cong and add needed configuration
  # Run container
  docker run -d -p 127.0.0.1:8889:80 --name inno-dkr-02 -v ${PWD}\nginx.conf:/etc/nginx/nginx.conf:ro nginx:stable 
```
### 2. Проверь работу, обратившись к 127.0.0.1:8889, – в ответ должно возвращать строку Welcome to the training program Innowise. Docker Выведи в консоли список запущенных контейнеров – контейнер должен быть запущен. Выполни подсчёт md5sum конфигурационного файла nginx командой: docker exec -ti inno-dkr-02 md5sum /etc/nginx/nginx.conf
```powershell
  curl http://127.0.0.1:8889
  # Check running containers
  docker ps
  # Calculate md5sum of config in container. An MD5 hash is a 32-character hexadecimal string (e.g., 098f6bcd4621d373cade4e832627b4f6) that uniquely represents the contents of a file.
  docker exec -ti inno-dkr-02 md5sum /etc/nginx/nginx.conf
  # I got this: 70cf706bb95a6fe69fce0ffd6fbd8601
```