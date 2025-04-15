# Task 1.1 VirtualBox, Linux, CLI. Работа с командной строкой, текстовые редакторы
### 1. Создать виртуальную машину Ubuntu на VirtualBox, используя официальный ISO образ.
### 2. Прочти/узнай отличия абсолютного пути от относительного, это важно. Посмотри абсолютный путь папки в которой ты находишься.
```bash
  pwd
```
### 3. Перейди на одну директорию ниже, вернись обратно.
```bash
  ls
  cd Desktop
  cd ..
```
### 4. Перейди в root директорию, сделай полный вывод содержимого (ls -l). После чего сделай полный вывод, включая скрытые файлы (ls -la). Разберись что означает каждая колонка в выводе.
```bash
  cd /
  ls -l
  ls -la
```
### 5. Перейди в свою домашнюю директорию. Создай файл “dog.txt”. С помощью команды “echo” запиши в файл название своего любимого фильма. 
```bash
  cd ~
  touch dog.txt
  echo "My favorite movie is Intouchables!" >> dog.txt
```
### 6. При помощи графического редактора vim добавь имена 4х главных героев в файл с новой строки.
```bash
  sudo apt install vim
  vim dog.txt
```
> Then click `i` to enter insert mode, press enter to go on the next line and write characters in the next line. Press `Esc` to exit insert mode. Type `:wq` and press `Enter` to save and quit.
### 7. Выведи содержимое файла с помощью команды “cat”.
```bash
  cat dog.txt
```
### 8. Переименуй файл с “dog.txt” в “cat.txt”. Создай новую директорию под названием “animals”, перемести “cat.txt” в неё.
```bash
  mv dog.txt cat.txt
  mkdir animals
  mv cat.txt animals
```
### 9. Находясь в директории “animals” выведи содержимое файла “cat.txt” используя относительный путь к файлу (также такое обращение к файлу называют “через точку”). Перейди в root директорию. Выведи содержимое файла “cat.txt” с помощью абсолютного и относительного путей.
```bash
  cat ./cat.txt
  cd /
  cat /home/vika/animals/cat.txt
  cat home/vika/animals/cat.txt
```
### 10. Создай еще несколько файлов с названиями животных в директории “animals”. Например “mouse.txt” и др.
```bash
  cd home/vika/animals
  touch mouse.txt cow.txt
```
### 11. Введи название своего любимого мультика в “mouse.txt” с помощю “>”. Разберись в чем разница между “>” и “>>”.
```bash
  echo "My favorite cartoon is Intouchables!" > mouse.txt
  cat mouse.txt
  echo "Tom is a cat." >> mouse.txt
```
> So when you use `>` content would be overwrite, but this `>>` adds new content at the end of file.
### 12. Найди в директории “animals” файл “cat.txt” с использованием команды “grep”. Далее сделай то же самое с помощью команды “find.”
```bash
  find -name "cat.txt"
  grep "movie" animals/*
```
### 13. Удали один из файлов в директории  “animals”. После чего удали директорию “animals”. Прочитай про варианты флагов у команды “rm”.
```bash
  cd animals
  rm cow.txt
  cd ..
  rm -r animals
```
### 14. Выведи историю своего взаимодействия с командной строкой и найди команду которую ты вводил для создания директории. (Используй pipe “|”)
```bash
  history
  history | grep mkdir
```
### 15. Используемые команды должны быть записаны в отдельный файлик.
```bash
  history > task1_1.txt
```
