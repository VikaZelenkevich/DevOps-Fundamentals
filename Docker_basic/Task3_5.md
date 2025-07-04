### 1. Запусти контейнер со следующими параметрами: должно работать в фоне,имеет имя inno-dkr-05-run-X, где X - набор из 10 случайных букв и/или цифр которая должна генерироваться в момент запуска контейнера (можно использовать команду cat /dev/urandom | tr -cd 'a-f0-9' | head -c 10),образ - nginx:stable,команда для запуска обоих контейнеров должна быть одинаковой (выполнить одинаковую команду два раза подряд).
```bash
  # Run containers twice
  docker run -d --name inno-dkr-05-run-$(cat /dev/urandom | tr -cd 'a-f0-9' | head -c 10) nginx:stable
  docker run -d --name inno-dkr-05-run-$(cat /dev/urandom | tr -cd 'a-f0-9' | head -c 10) nginx:stable
```
### 2. Запусти контейнер со следующими параметрами:должно работать в фоне,имеет имя inno-dkr-05-stop,образ - nginx:stable.
```bash
  # Run container
  docker run -d --name inno-dkr-05-stop nginx:stable
```
### 3. Выполни команду docker ps, вывод перенаправь в файл /home/user/ps.txt (docker ps | tee /home/user/ps.txt). Останови контейнер inno-dkr-05-stop. Выведи список всех контейнеров. Одной командой останови все запущенные контейнеры. Выведи список всех контейнеров. Одной командой удали все контейнеры, любой из разобранных методов подходит.
```powershell
  docker ps | tee D:/DevOps_Fundamentals/ps.txt
  # Stop container
  docker stop inno-dkr-05-stop
  docker ps -a
  # Stop all running containers at once, docker ps -aq: give all container IDs 
  docker stop $(docker ps -aq)
  # Remove all containers at once
  docker rm $(docker ps -aq)
```