# Task 1.4 Network utilities. Системы и утилиты для работы с сетями в Linux
### 1. Создайте сервер t2.micro (Ubuntu), он должен иметь публичный IP-адрес и доступ в Интернет. Также можно использовать две виртуальные машины (Ubuntu), поднятые на одном компьютере. Либо виртуальную машину и основной компьютер (если ОС вашего основного компьютера Ubuntu). Чтобы два хоста были подключены к одной сети и имели доступ друг к другу. Далее в заданиях будет использована терминология основного хоста и удаленного хоста. Нет особой разницы, какую ты сделаешь основной, главное, чтобы далее ты не путался в формулировках.
```  
After creation of the second VM (remote VM), for both VM go to settings -> Network -> Adapter 2 set Attached to: Host-only Adapter, Promiscuos Mode: Allow VM
```
```bash
  # for main and remote host find IP using:
  hostname -I
  # Test Connectivity from main host to remote host:
  ping 192.168.56.101
```
### 2. Проверьте доступность интернета на главном хосте, воспользовавшись командой ping. Для этого нужно пропинговать доверенный адрес в Интернете, который точно будет доступен (например, google.com). Запиши вывод в файл.
```bash
  # -c mean count, stop after 4 pings
  ping -c 4 google.com > result.txt
  cat result.txt
```
### 3. Выполните команду трассировки до доверенного адреса в Интернете, который точно будет доступен (например, google.com). Изучи вывел команду. Запиши вывод в файл.
```bash
  sudo apt update
  sudo apt install traceroute
  # tee writes output both to the terminal and to the file
  traceroute google.com | tee traceroute_output.txt
```
### 4. Узнай IP-адрес основного и удаленного хостов. Посмотри, какие сетевые интерфейсы у тебя использовались.  
```bash
  # This command show all IP
  hostname -I 
  # This command show all IP and  in brief (simplified) format 
  ip -br a  
  # network interface for my main and remote hosts: enp0s3, enp0s8
```
### 5. Проверьте статус межсетевого экрана на удаленном хосте. Используй команду «sudo ufw status». В случае, если он включен, отключите его.
```bash
  sudo ufw status
  sudo ufw disable
```
### 6. Установи себя на основном хосте Wireshark. Запустите его, ознакомьтесь с интерфейсом.
```bash
  sudo apt update
  sudo apt install wireshark
  # add my user to the wireshark group
  sudo usermod -aG wireshark vika
  # Run Wireshark
  wireshark

```
### 7. Установи на удаленном хосте telnet и подключись по telnet к локальному хосту (в случае с AWS тебе необходимо будет создать нового юзера на удаленном хосте под кредитами, к которым ты будешь подключаться).
```bash
  # On the remote host
  sudo apt update
  sudo apt install telnet
  telnet 192.168.56.101
  # For me it show: Unable to connect to remote host: Connection refused. So i did this:
  # On the main host
  # Enable the firewall and Allow port 23
  sudo ufw enable
  sudo ufw allow 23/tcp
  sudo ufw reload
  # Install the Telnet server, if not installed 
  which telnetd
  sudo apt update
  sudo apt install telnetd
  # Verify that port 23 is listening
  sudo ss -tlnp | grep :23
  # For me nothing was shown, so i open this file with a text editor, where i find this string: #<off># telnet	stream	tcp	nowait	root	/usr/sbin/tcpd	/usr/sbin/telnetd. So i delete #<off># and Save.
  sudo nano /etc/inetd.conf
  # Restart inetd
  sudo systemctl restart inetd
  # again check if port 23 in the LISTEN state
  sudo ss -tlnp | grep :23
  # On the remote host
  telnet 192.168.56.101
```
### 8. Запустите Wireshark на главном хосте. При запущенном Wireshark введите команду «umane -a» в терминале с открытой telnet-сессией. Отыщи информацию комиссию по протоколу telnet в Wireshark. Проанализируй, что ты видишь.
```bash
  # On the remote host
  telnet 192.168.56.101
  # After that you write login and password of main host and you are inside a Telnet session.
  uname -a 
```
### 9. Выключите или удали telnet на удаленном хосте.
```bash
  # On the main host
  sudo systemctl stop inetutils-inetd
  sudo systemctl status inetutils-inetd
  # To ensure that Telnet doesn't start automatically after a reboot, you should disable the service:
  sudo systemctl disable inetutils-inetd
```
### 10. Далее повторяем то же самое, однако для подключения к удаленному хосту по ssh. Сравните результаты анализа. Запишите свои выводы (логические выводы) в файл.
```bash
  # On the remote host
  sudo apt update
  sudo apt install openssh-server
  # Check that it's running
  sudo systemctl status ssh
  # If it’s not active, start and enable it
  sudo systemctl enable --now ssh
  # On the main host
  sudo ufw allow ssh
  # Check if port 22 (ssh default) allowed in firewall
  sudo ufw status
  # On the remote host 
  ssh user@main-ip
  # For me ssh vika@192.168.56.101
```
### 11. Включите ufw и настройте запрет на подключение по 22 порту на удаленном хосте. Попробуйте подключиться к основному хосту, удаленному по ssh.
```bash
  # On the main host
  sudo ufw enable
  sudo ufw deny 22/tcp
  # Check if port 22 (ssh default) denied in firewall
  sudo ufw status
  # On the remote host 
  ssh user@main-ip
  # For me ssh vika@192.168.56.101. And it doesn't connect to main host
```
### 12. Скачай nmap на основной хост и сделай полную проверку удаленного хоста. На удаленном хосте введи команда «sudo ss -tuln». Запиши оба результата в файл. Проанализируйте сходство обоих выводов.
```bash
  # On the main host
  sudo apt update
  sudo apt install nmap
  # -sS: TCP SYN scan. -p-: Scan all 65535 ports. -oN nmap_scan.txt: Save output to a file.
  sudo nmap -sS -p- remote_ip -oN nmap_scan.txt
  # On the remote host
  sudo ss -tuln >> ss_scan.txt
```