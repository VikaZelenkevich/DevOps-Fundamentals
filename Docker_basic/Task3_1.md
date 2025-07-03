# Task 3.1 Basics. Знакомство с Docker
### 1. Установи Docker на свою систему, как указано в официальной документации. Запусти запись вывода терминала в файл. Запусти контейнер на порту 28080 из официального образа nginx: docker run -d -p 127.0.0.1:28080:80 --name rbm-dkr-01 nginx:stable. Выведи в консоли список запущенных контейнеров командой: docker ps. При помощи утилиты curl запроси адрес http://127.0.0.1:28080 - должна появиться приветственная страница nginx. Если не появляется, то запуск контейнера неуспешен по какой-то причине.
```powershell
  docker --version
  # Start recording terminal output to a file
  Start-Transcript -Path ./docker-log.txt
  # When you open http://localhost:28080 in your browser, It goes to your machine's port 28080, Docker forwards that traffic into the container’s port 80,And Nginx handles the request inside the container.
  docker run -d -p 127.0.0.1:28080:80 --name rbm-dkr-01 nginx:stable
  # Check if the container is running
  docker ps
  # Check NGINX with curl
  curl http://127.0.0.1:28080
```
### 2. Останови ранее запущенный контейнер командой: docker stop rbm-dkr-01. Выполни команду docker ps, тем самым проверь произошедшие изменения в списке запущенных контейнеров - список контейнеров должен быть пуст. При помощи утилиты curl запроси адрес http://127.0.0.1:28080 - сейчас должна быть ошибка, поскольку мы уже удалили контейнер и ничего не слушается на этом порту.
```powershell
  docker stop rbm-dkr-01
  # Verify stopped container list
  docker ps   # Should be empty
  curl http://127.0.0.1:28080   # Should return error
```
### 3. При помощи команды docker ps -a выведи список всех контейнеров в системе - в списке будет твой остановленный контейнер. Останови запись в файл и загрузите полученные логи в репозиторий на gitlab.
```powershell
  # Show all containers, including stopped
  docker ps -a
  # Stop logging
  Stop-Transcript
  # Upload file to GitLab
  git init
  git remote add origin https://devops-gitlab.inno.ws/viktoryia.zeliankevich/docker-task1.git
  git add docker-log.txt
  git commit -m "Added Docker terminal session log"
  git push -u origin master
```