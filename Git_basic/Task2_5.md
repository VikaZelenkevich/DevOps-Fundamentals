# Task 2.5 Работа с историей и удаленными репозиториями
### 1. Клонируй репозиторий: https://devops-gitlab.inno.ws/ilya.sergienko1/git-squash. Создай в своем аккаунте репозиторий devops-task-force. Залей весь репозиторий git-squash в devops-task-force. После этого в ветке develop используй squash для объединения всех коммитов в один. После этого попробуй выполнить git push в ваш репозиторий. Сохрани текст ошибки. Выполни git push --force в ваш репозиторий.
```powershell
  # Clone repository
  git clone https://devops-gitlab.inno.ws/ilya.sergienko1/git-squash
  cd git-squash
  git remote add force https://devops-gitlab.inno.ws/viktoryia.zeliankevich/devops-task-force.git
  # Push repository
  git push -u force --all 
  git branch -a 
  # I see only main branch so in task I will use main branch and not develop
  # See commit history in main branch 
  git log --oneline
  # Rebase and squash FIX commits into Initial commit
  git rebase -i --root
  git push -u force --all 
  # I got this error:
  # ! [rejected]        main -> main (non-fast-forward)
  # error: failed to push some refs to 'https://devops-gitlab.inno.ws/viktoryia.zeliankevich/devops-task-force.git'
  # hint: Updates were rejected because the tip of your current branch is behind
  # hint: its remote counterpart. If you want to integrate the remote changes,
  # hint: use 'git pull' before pushing again.
  # hint: See the 'Note about fast-forwards' in 'git push --help' for details.
  git push -u force --all --force
  # Got similar error
```
### 2. Клонируй репозиторий: git-rebase. Создай новый репозиторий у себя на gitlab и запушь туда репозиторий. Создай 2 pull requests: из ветки feature в develop и из ветки develop в master.
```powershell
  # Clone repository
  git clone https://devops-gitlab.inno.ws/devops-board/git-rebase.git
  cd git-rebase
  git remote add rebase https://devops-gitlab.inno.ws/viktoryia.zeliankevich/devops-task-rebase.git
  # Fetch from all remotes (especially rebase) to ensure all branches are visible:
  git fetch --all
  # Checkout a remote-tracking branch, Git create a local branch that tracks it
  git checkout develop
  git checkout feature
  git push -u rebase --all   
```
### 3. Клонируй репозиторий: https://devops-gitlab.inno.ws/ilya.sergienko1/git-merge. Создай новый репозиторий у себя на gitlab и загрузи туда репозиторий. Создай pull request из ветки feature в ветку develop. Посмотри, как отображаются конфликты в интерфейсе gitlab.
```powershell
  # Clone repository
  git clone https://devops-gitlab.inno.ws/ilya.sergienko1/git-merge
  cd git-merge
  # See all branches that attached to repository
  git branch -a 
  git remote add merge https://devops-gitlab.inno.ws/viktoryia.zeliankevich/devops-git-merge.git 
  # Checkout a remote-tracking branch, Git create a local branch that tracks it
  git checkout development
  git checkout feature
  git push -u merge --all 
```
### 4. Выполни fork репозитория git-checkout. Переключись в ветку feature и добавь в нее новый файл с интересной информацией отдельным коммитом. Создай pull request в основной проект: git-checkout.
```powershell
  # Clone repository
  git clone https://devops-gitlab.inno.ws/viktoryia.zeliankevich/git-checkout
  cd git-checkout
  # Move to feature branch
  git checkout feature
  echo "Interesting info" > cool-info.txt
  git add info.txt
  git commit -m "Add interesting info file"
  git push origin feature
```