# Git useful commands
## Работа с branch
### Удаление branch
```
git branch -d BRANCH_NAME
```
### Удаление branch на remote server
```
git push origin --delete BRANCH_NAME
```
### Переименование branch
```
git branch -m OLD_NAME NEW_NAME
```

## Модификация коммитов
### Если сделали коммит и хотим изменить commit message
```
git commit --amend
```

### Если сделали коммит и хотим поменять автора
```
git commit --amend --author="AUTHOR_NAME <email@address.com>"
```

### Если сделали коммит и хотим добавить файл, или внести изменения в добавленные файлы
С изменением commit message.
```
git add (files)
git commit --amend
```
Без изменения commit message.
```
git add (files)
git commit --amend --no-edit
```
Изменение конкретного (непоследнего) commit message.
```
git rebase --interactive [NUM_VERSION]
// Change pick -> edit
git commit --amend
git rebase --continue
```

### Объединение нескольких коммитов
!!! перед любыми подобными действиями лучше делать BACKUP
```
git reset HEAD^
git add (files)
git commit --amend
```
Если после интересующих нас коммитов были совершены новые, то перед выполнением необходимо просто сделать новый бранч, перейти к интересующему нас коммиту и выполнить последовательность, после чего подтянуть более новые изменения.

## Stash
### Сохранение всех незакомиченых модификаций в хранилище
```
git stash
```
### Восстановление модификаций из хранилища
```
git stash apply
```
### Просмотр списка сохраненных стешей
```
git stash list
```
### Просмотр diff файла конкретного стеша с его опорным комитом
```
git stash show -p stash@{NUMBER}
```
Где __NUMBER__ -- номер интересеющего нас стеша.

Если хотим сохранить данный diff файл, то можно просто перенаправить stdout команды в файл.
```
git stash show -p stash@{NUMBER} > FILE.diff
```

## Diff
### Diff между текущим состоянием и последним комитом
```
git diff
```
### Diff между коммитами
```
git diff old_commit new_commit
```
### Показать только названия измененных файлов
```
git diff --name-only old_commit new_commit
```

## Patch
### Применение патчей
Для применения патча достаточно выполнить следующую команду, где -v - сделает подробный вывод, file.diff - файл, содержащий патч, который к примеру мог быть сгенерирован __git diff__ или unix утилитой __diff__. Патч либо будет полностью применён, либо ничего не будет изменено.
```
git apply -v file.diff
```
### Модификация патчей
С помощью __git format-patch__ подготавливаем набор всех коммитов для текущей ветки, на которые она отличается от базовой. С помощью __sed__ модифицируем патчи. __Git am__ применяет патчи вместе с коммитами.
```
git format-patch [BASE_BRANCH_NAME] -o [OUTPUT_DIRECTORY]
find . -name "*.patch" -exec sed -i '' -e 's/OLD_TEMPLATE/NEW_TEMPLATE/g' {} \;
git reset --hard [BASE_BRANCH_NAME]
git am *.patch
```

## Удалённый репозиторий
### Список удалённых репозиториев
```
git remote
```


### Получение URL удалённых репозиториев
```
git remote -v
```


### Добавление нового репозитория в список
```
git remote add [NAME_REMOTE] [URL]
```
### Удаление branch на удалённом репозитории
```
git push [NAME_REMOTE_REPO] --delete [NAME_BANCH_ON_REMOTE_REPO]
```

## Pull request
Сначала добавляем репозиторий в список remote репозиториев:
```
git remote add [name] [URL]
```
После чего отправляем запрос
```
git push [name] [branch]
```

## Разрешение конфликтов
### Переименование файла
Для устранения конфликта с переименованным файлом необходимо исправить связанные с ним файлы, удалить старый, добавить новый и модифицированные.
```
git rm old_name
git add new_name
git add mod_files
git rebase --continue
```

## Удаление файлов из истории
```
git filter-repo --invert-paths --path [PATH_TO_DIRECTORY]
git remote add origin [ORIGIN_URL]
git push origin --force --all
git push origin --force --tags
```

