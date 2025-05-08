# Task 1.5 Процессы, systemd и cron
### 1. Написать Systemd unit, который будет раз в 15 секунд писать в файл вывод команды uptime. При убийстве процесса он должен перезапускаться. 
```bash
  sudo nano /usr/local/bin/uptime-logger.sh
```
#### Paste this:
```bash
#!/bin/bash

while true; do
  uptime >> /var/log/uptime.log
  sleep 15
done
```
```bash
  sudo touch /var/log/uptime.log
  # Make it executable:
  sudo chmod +x /usr/local/bin/uptime-logger.sh
  # Create a systemd service unit:
  sudo nano /etc/systemd/system/uptime-logger.service
```
#### Paste this:
```
    [Unit]
    Description=Log system uptime every 15 seconds

    [Service]
    ExecStart=/usr/local/bin/uptime-logger.sh
    Restart=always
    RestartSec=5

    [Install]
    WantedBy=multi-user.target
```
```bash
  # Reload Systemd & Start the Service:
  sudo systemctl daemon-reexec
  sudo systemctl daemon-reload
  sudo systemctl enable --now uptime-logger.service
  # Check that Logs output write to /var/log/uptime.log.
  cat /var/log/uptime.log
  # Search for process ID (PID) by name
  ps aux | grep uptime
  sudo kill PID
```
### 2. Если значение Load Average за минуту больше 1, то вывод команды uptime должен записываться в файл overload.
```bash
  sudo nano /usr/local/bin/uptime-logger.sh
```
#### Paste this:
```bash
#!/bin/bash

while true; do
  uptime_output=$(uptime)
  echo "$uptime_output" >> /var/log/uptime.log

  # Extract 1-minute load average
  load1=$(echo "$uptime_output" | awk -F'load average: ' '{print $2}' | cut -d',' -f1)

  # Compare: if load1 > 1
  if (( $(echo "$load1 > 1.0" | bc -l) )); then
    echo "$uptime_output" >> /var/log/overload.log
  fi

  sleep 15
done

```
```bash
  # Creates the file. chmod 644 gives read/write permission to root, read-only to others (standard for logs).
  sudo touch /var/log/overload.log
  sudo chmod 644 /var/log/overload.log
  # Restart Service:
  sudo systemctl restart uptime-logger.service
  # Check that Logs output write to /var/log/overload.log
  cat /var/log/overload.log
```
### 3. Установить утилиту stress. Нагрузить свою систему, применив команду stress --cpu x --timeout 50s, где x - количество процессоров у виртуалки/системы. Проверить работоспособность 2 пунтка.
```bash
  # Check how many processors your system has
  nproc
  sudo apt update
  sudo apt install stress
  # System has 4 processors so:
  stress --cpu 4 --timeout 50s
  # Check that Logs output write to /var/log/overload.log after stress
  cat /var/log/overload.log
```
### 4. Когда файл overload достигает размера в 50 кб, то он должен очищаться. В файл cleanup должны складываться логи об успешных очистках с временем самой очистки.
```bash
  sudo nano /usr/local/bin/uptime-logger.sh
```
#### Paste this:
```bash
#!/bin/bash

while true; do
  uptime_output=$(uptime)
  echo "$(date): $uptime_output" >> /var/log/uptime.log

  # Extract 1-minute load average
  load1=$(echo "$uptime_output" | awk -F'load average: ' '{print $2}' | cut -d',' -f1)

  # Compare load average to 1.0
  if (( $(echo "$load1 > 1.0" | bc -l) )); then
    echo "$(date): $uptime_output" >> /var/log/overload.log
  fi

  # Check overload file size in KB
  if [ -f /var/log/overload.log ]; then
    size_kb=$(du -k /var/log/overload.log | cut -f1)
    if (( size_kb >= 50 )); then
      echo "$(date): overload.log cleared at ${size_kb} KB" >> /var/log/cleanup.log
      > /var/log/overload.log
    fi
  fi

  sleep 15
done
```
```bash
  # Creates the file. chmod 644 gives read/write permission to root, read-only to others (standard for logs).
  sudo touch /var/log/cleanup.log
  sudo chmod 644 /var/log/cleanup.log
  # Restart Service:
  sudo systemctl restart uptime-logger.service
  # Check that Logs output write to /var/log/cleanup.log
  cat /var/log/cleanup.log
```
### 5. Написать cron job, проверяющий каждые 10 минут статус вышесозданного юнита.
```bash
  sudo crontab -e
  */10 * * * * systemctl is-active uptime-logger.service >> /var/log/uptime-check.log 2>&1
  # Creates the file. chmod 644 gives read/write permission to root, read-only to others (standard for logs).
  sudo touch /var/log/uptime-check.log
  sudo chmod 644 /var/log/uptime-check.log
  # Restart Service:
  sudo systemctl restart uptime-logger.service
  # Check that Logs output write to /var/log/cleanup.log
  cat /var/log/uptime-check.log
```
### 6. Запустить утилиту ping, вывести запущенный процесс в background, проверить, что процесс находится в фоновом режиме, вернуть процесс в foreground. Остановить процесс, после чего удалить его. 
```bash
  ping google.com &
  # Verify it's running in the background
  jobs
  # Bring it to the foreground
  fg %1
  # Stop the process (in foreground)
  Ctrl + C
```
### 7. Используемые команды должны быть записаны в отдельный файлик. 
