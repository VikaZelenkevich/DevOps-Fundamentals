# Task 1.2 Конфигурация виртуальной машины
### 1. Поднять ubuntu на VirtualBox/VMware/Cloud. Создать там пользователей Raymond и John. 
```bash
  sudo useradd -m -d /home/Raymond -g users -s /bin/bash Raymond
  sudo useradd -m -d /home/John -g users -s /bin/bash John
```
### 2. Raymond должен заходить только по SSH ключу на инстанс. А John должен каждый раз вводить свой пароль при попытке зайти на инстанс и не иметь возможности зайти по ssh ключу.
#### In local machine in powershell: 
```powershell
  ssh-keygen -t rsa -b 4096
```
#### In ubuntu terminal: 
```bash
  sudo mkdir -p /home/Raymond/.ssh
  # Using nano editor we paste public key content
  sudo nano /home/Raymond/.ssh/authorized_keys
  sudo chmod 700 /home/Raymond/.ssh
  sudo chmod 600 /home/Raymond/.ssh/authorized_keys
  sudo nano /etc/ssh/sshd_config
  # Set password for John
  sudo passwd John
```
#### In nano editor paste this: 
```  
  Match User Raymond
    PasswordAuthentication no
    PubkeyAuthentication yes
    AuthenticationMethods publickey
  Match User John
    PasswordAuthentication yes
    PubkeyAuthentication no
    AuthenticationMethods password
```
#### In local machine in powershell we can check connection to Raymond and John: 
```powershell
  ssh -i C:\Users\User\.ssh\id_rsa Raymond@Raymond_Ip
  ssh John@John_Ip
```
### 3. Raymond должен иметь доступ к sudo, а John не сможет использовать его. 
```bash
  # Add the user Raymond to the sudo group
  usermod -aG sudo Raymond 
  # Check that Raymond has sudo access and John hasn't by running the command:
  sudo -l -U Raymond
  sudo -l -U John
```

### 4. Из под пользователя Raymond создать документ, используя права доступа сделать так, чтобы пользователь John не смог редактировать содержимое документа, но смог его прочитать. Из под пользователя John создать shell скрипт(с любой командой) и дать возможность запускать его пользователю Raymond. 
```bash
  # From user Raymond
  touch docs.txt
  echo "Text" > /home/Raymond/docs.txt
  chmod 644 /home/Raymond/docs.txt
  # Check that John can only read docs.txt, from user John
  su John
  cat /home/Raymond/docs.txt
  echo "test" >> /home/Raymond/docs.txt
  # Create shell script, here -e is tells echo to interpret escaped characters like \n (newline), #!/bin/bash is a shebang. It tells the system to run the script using the Bash shell.
  echo -e '#!/bin/bash\necho "Hi, John!"' > /home/John/hello.sh
  # Change file permissions (add execute permission)
  chmod +x /home/john/hello.sh
  # Cause users John, Raymond and Vika are in same group (users), add new group
  su vika
  sudo groupadd scriptusers
  sudo usermod -aG scriptusers John
  sudo usermod -aG scriptusers Raymond
  # Set group ownership of the script
  sudo chown John:scriptusers /home/John/hello.sh
  sudo chmod 750 /home/John/hello.sh
  # Check that script running from users Raymond and John
  /home/John/hello.sh
```
### 5. Поменять интерпретатор по умолчанию пользователю John(сделать, чтобы был bash). Создать нового пользователя так, чтобы у него сразу был интерпретатор sh по умолчанию. 
```bash
  sudo chsh -s /bin/bash John
  sudo useradd -m -d /home/Kate -g users -s /bin/sh Kate
```
### 6. Добавить нового пользователя в группу пользователя John, проверить, сможет ли он запускать созданный ранее shell скрипт. 
```bash
  sudo usermod -aG scriptusers Kate
  sudo passwd Kate
  su Kate
  /home/John/hello.sh
```
### 7. Создать на своей хостовой машине файл, после чего прокинуть его на инстанс с помощью scp. 
```bash
  su vika
  echo "This file from Ubuntu." > /home/vika/text.txt
```
#### In local machine in powershell: 
```powershell
  scp -i C:\Users\User\.ssh\id_rsa vika@vika_Ip:/home/vika/text.txt C:\Users\User
  # Check file content
  Get-Content C:\Users\User\text.txt
```
### 8. Используемые команды должны быть записаны в отдельный файлик