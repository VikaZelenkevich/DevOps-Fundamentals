# Task 1.3 Утилиты обработки текста/логов
### 1. Вывести размеры разделов диска в отдельный файл. Отсортировать количество столбцов до трех, оставив только Filesystem, Use%, Mounted on.
```bash
  # df -h — show disk usage in human-readable format, awk — filters the output: NR==1 — keep the header line, /^\/dev/ — include lines starting with /dev, print $1, $5, $6 — show 1,5,6 columns
  df -h | awk 'NR==1 || /^\/dev/ {print $1, $5, $6}' > disk_size.txt
```
### 2. Узнать размер всех файлов и папок в директории /etc. Отсортировать вывод так, чтобы показывало только 10 самых больших файлов.
```bash
  # -exec du -h {} + — for each file, execute (run) du -h (get size in human-readable format) ({} — each found file or folder goes here, + — Run the command once with as many files as possible, 2>/dev/null —  suppress any permission errors, sort -hr — sort by size (-h = human-readable, -r = reverse: biggest first), head -n 10 — show only the top 10 largest files
  find /etc -type f -exec du -h {} + 2>/dev/null | sort -hr | head -n 10
```
### 3. Cоздать файл со следующим содержанием.
NDS/A  
NSDA  
ANS!D  
NAD/A  
```bash
  touch file.txt
  nano file.txt
```
### 4. Вывести строки NDS/A и NAD/A из файла используя awk или sed(regexp). 
```bash
# NR is the built-in variable for the current line number. If the current line is 1 or 4, then prints the entire line ($0 means "the whole line").
  awk 'NR==1 || NR==4 {print $0}' file.txt
  sed -n '1p;4p' file.txt
```
### 5. Вывести пронумерованные строчки из /etc/passwd, в которых есть оболочка /bin/bash, и перенаправить вывод в файл. 
```bash
  cd ~
  touch bash_users.txt
  # -F: tells awk to use : as the field separator instead of the default (which is whitespace).
  awk -F: '/\/bin\/bash/ {print NR, $0}' /etc/passwd > /home/vika/bash_users.txt
```
### 6. Заменить все строчки с /bin/sh на /bin/bash. Проводить на заранее скопированном файле passwd((НЕ ОРИГИНАЛЕ, иначе будут проблемы(обсудить какие и для чего нужен этот файл с ментором)), просто скопируйте его и переименуйте в passwd.bak).
```bash
  cp /etc/passwd ~/passwd.bak
  sed -i 's/\/bin\/sh/\/bin\/bash/g' ~/passwd.bak
```
### 7. Используемые команды должны быть записаны в отдельный файлик.