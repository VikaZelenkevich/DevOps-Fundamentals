# Task 2.7 Configs
### 1. Проверить наличие файла{{ ~/.gitconfig}}(только на Linux системе, файл будет присутствовать, если до этого ты настраивал git config --global). В случае отсутствия можешь создать его сам.
```powershell
  # Check if ~/.gitconfig exist
  Test-Path "$HOME\.gitconfig"
  # To view the contents of .gitconfig in PowerShell
  Get-Content "$HOME\.gitconfig"
```
### 2. Настроить .gitconfig так, чтобы он подключал другой конфиг для определенной директории. Ниже мы для всех подпапок и файлов директории ~/innowise/ подключаем конфиг .gitconfig-innowise
- [includeIf "gitdir:~/innowise/"]
- path = ~/.gitconfig-innowise
```powershell
  # Add the following to the end of ~/.gitconfig. This tells Git to include a custom config if you're working inside any repo under ~/innowise/.
  Add-Content "$HOME\.gitconfig" '[includeIf "gitdir:~/innowise/"]'
  Add-Content "$HOME\.gitconfig" 'path = ~/.gitconfig-innowise'
```
### 3. Написать сам .gitconfig-innowise. Здесь мы просто добавляем нужные нам креденшиалы.
- [user]
- name = Аркадий Паровозов
- email = arkadiy_parovozov@innowise-group.com
```powershell
  # Create .gitconfig-innowise with user details
  Set-Content "$HOME\.gitconfig-innowise" "[user]"
  Add-Content "$HOME\.gitconfig-innowise" "name = Arkady Parovozov"
  Add-Content "$HOME\.gitconfig-innowise" "email = arkadiy_parovozov@innowise-group.com"
```
### 4. Теперь дело за малым. Создай репозиторий внутри директории ~/innowise/, после чего внеси какие-нибудь изменения и закоммить. Если автором коммита является Аркадий Паровозов, то ты справился.
```powershell
  # Create branch and switch to it
  New-Item -Path "$HOME\innowise\demo-repo" -ItemType Directory
  Set-Location "$HOME\innowise\demo-repo"
  git init
  #  Add file and commit:
  "Hello" | Out-File test.txt
  git add test.txt
  git commit -m "Arkady's test commit"
  # Check commit author:
  git log
  # the author is Arkady Parovozov
```