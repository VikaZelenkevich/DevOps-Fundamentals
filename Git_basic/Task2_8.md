# Task 2.8 Git flow 
### 1. Создай новый локальный репозиторий. Создай две ветки: master/develop. Добавь пару коммитов в master. Перенеси изменения в ветку develop. Добавь один коммит в ветку develop. Создай из ветки develop новую ветку feature. Создай по одному коммиту в ветках develop и feature. Загрузи все изменения на gitlab.
```bash
  mkdir devops-task8
  cd devops-task8
  git init
  # Create files and add commits:
  echo "First update" > file.txt
  git add .
  git commit -m "Initial commit on master"
  echo "Second update on master" >> file.txt
  # flag "-a" Adds modified, tracked files to the commit. New files still need git add before you can commit them
  git commit -am "Second commit on master"
  # Create develop branch from master
  git checkout -b develop
  echo "Change in develop" >> file.txt
  git commit -am "Commit on develop"
  # Create feature branch from develop
  git checkout -b feature
  echo "Feature work" >> feature.txt
  git add .
  git commit -m "Commit on feature"
  # Switch to develop and make another commit
  git checkout develop
  echo "Another develop change" >> file.txt
  git commit -am "Second develop commit"
  # Add Remote Gitlab repo
  git remote add origin https://devops-gitlab.inno.ws/viktoryia.zeliankevich/devops-task8.git
  # Push all branches to GitLab
  git push -u origin master
  git push -u origin develop
  git push -u origin feature

```
### 2. Создай pull request из feature в develop и замержи его. Создай pull request из develop в master и замержи его. Пометь замерженный коммит в ветке master тегом release_1.1 и загрузи в свой репозиторий.
```bash
  # Get latest commit on master
  git checkout master
  git pull origin master
  git log --oneline -1
  # Tag the last commit
  git tag release_1.1
  git push origin release_1.1
```