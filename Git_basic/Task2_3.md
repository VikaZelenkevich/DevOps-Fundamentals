# Task 2.3 Мерж и конфликты
### 1. Объедини ветку develop с веткой master из предыдущего задания. Запушь ветку в удаленный репозиторий.
```powershell
  # Checkout the master branch
  git checkout master
  # Pull latest from remote to avoid divergence
  git pull origin master
  # Merge develop into master
  git add .
  git commit -m "Add all files"
  # Push merged master to remote:
  git push origin master
```
### 2. Клонируй репозиторий: https://devops-gitlab.inno.ws/ilya.sergienko1/git-merge. Смержи ветку feature в ветку develop, устранив конфликты. При разрешении конфликта нужно выбрать последнее по времени изменение в ветках develop и feature по выводу git log.
```powershell
  # Clone fresh copy
  git clone https://devops-gitlab.inno.ws/ilya.sergienko1/git-merge.git
  cd git-merge
  # Checkout feature branch
  git checkout feature
  # Checkout development branch
  git checkout development
  # Pull latest changes
  # git pull origin development
  # Merge feature into development
  git merge feature
  git add .
  git commit -m "Solve merge conflict"
  
```
### 3. Смержи получившуюся ветку develop в ветку master. При разрешении конфликта нужно выбрать последнее по времени изменение в ветках develop и master, исключая при сравнении времени только что сделанный merge коммит, который появился в ветке develop после выполнения шага 2.
```powershell
  # Switch to master
  git checkout master
  # Pull latest
  git merge development
```
### 4. Создай в своем аккаунте в gitlab репозиторий devops-task-merge и загрузи все изменения в него.
```powershell
  # Push local repo to your new GitLab repo:
  git remote add task2_3 https://devops-gitlab.inno.ws/viktoryia.zeliankevich/devops-task-merge.git
  # Push all branches
  git push -u task2_3 --all
```