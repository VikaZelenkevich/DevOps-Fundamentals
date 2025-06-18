# Task 2.4 Rebase, Squash и Cherry-pick
### 1. Клонировать репозиторий: git-rebase. Переместить коммиты из ветки feature в ветку develop с помощью rebase. Переместить коммиты из ветки develop в ветку master с помощью rebase. Создай репозиторий в своем аккаунте и загрузи в него получившийся локальный репозиторий. 
```powershell
  # Clone repository
  git clone https://devops-gitlab.inno.ws/devops-board/git-rebase
  cd git-rebase
  # See all branches that attached to repository
  git branch -a 
  # Checkout a remote-tracking branch, Git create a local branch that tracks it
  git checkout develop
  git checkout feature
  # Moving commits from the feature branch to the develop branch
  git rebase develop
  
  git checkout develop
  # Moving commits from the develop branch to the master branch
  git rebase master
```
### 2. Клонируй репозиторий: https://devops-gitlab.inno.ws/ilya.sergienko1/git-squash. Просмотри историю коммитов в ветке develop. Объедини все FIX-коммиты c родительским (Simplified sum function) в один коммит. Создай репозиторий в своем аккаунте и загрузите в него получившийся репозиторий. 
```powershell
  cd ..
  # Clone repository
  git clone https://devops-gitlab.inno.ws/ilya.sergienko1/git-squash.git
  cd git-squash
  # See all branches that attached to repository
  git branch -a 
  # I see only main branch so in task I will use main branch  and not develop
  # See commit history in main branch 
  git log --oneline
  # I get this:
  # a60ddff (HEAD -> main, origin/main, origin/HEAD) third fix commit
  # 70774aa second fix commit
  # 5e166bf first fix commit
  # abac525 Initial commit

  # Rebase and squash FIX commits into Initial commit
  git rebase -i --root
  # In vim editor click "i" and change strings:
  # pick abac525 Initial commit
  # squash 5e166bf first fix commit
  # squash 70774aa second fix commit
  # squash a60ddff third fix commit
  # Click "Esc" and write :wq -> Enter
  git log --oneline
  # I get this: 65a3600 (HEAD -> main) Initial commit
  git remote add squash https://devops-gitlab.inno.ws/viktoryia.zeliankevich/git-squash.git
  # Push repository
  git push -u squash --all  
```
### 3. Клонируй репозиторий: https://devops-gitlab.inno.ws/ilya.sergienko1/git-cherry-pick/. Перенести коммит "Formatted code" в ветку master. Создай репозиторий в своем аккаунте и залейте туда получившийся репозиторий.
```powershell
  cd ..
  # Clone repository
  git clone https://devops-gitlab.inno.ws/ilya.sergienko1/git-cherry-pick.git
  cd git-cherry-pick
  # See all branches that attached to repository
  git branch -a 
  # We have main and develop branches. Create master branch:
  git branch master
  git checkout main
  # This will return commit (from any branch) whose message contains the phrase "Formatted code"
  git log --oneline --all | Select-String "Formatted code"
  # We get commit hash with phrase: e4a9855 Formatted code
  git checkout master
  git cherry-pick e4a9855
  git remote add cherrypick https://devops-gitlab.inno.ws/viktoryia.zeliankevich/git-squash.git
  # Push repository
  git push -u cherrypick --all   

```
